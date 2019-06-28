---
title: "Spartan Messenger"
excerpt: "GRPC based terminal messaging system with encryption and LRU caching."
header:
  image: /assets/images/grpc-messenger.png
  teaser: assets/images/grpc-image.png
sidebar:
  - title: "Role"
    image: /assets/images/dennis-simon.jpg
    image_alt: "logo"
    text: "Coder"
  - title: "Responsibilities"
    text: "Wrote code for GRPC terminal messaging. "

---

## Technologies

Python, LRU Caching, AES encryption, GRPC


### Overview

+ Command line based message communication between multiple clients through central server using GRPC.
+ AES message encryption between clients to hide messages passing through server.
+ LRU caching to store recent messages in memory.
+ Preserves messages for users through sessions.
+ Message limiting system.
+ Group chat support with user login straight to group.
+ Import groups with client names from YAML file.
+ [Github](https://github.com/DVSimon/Spartan-Messenger)



### GRPC Proto creation

+ Setup pb2 and pb2_grpc using proto code
```python
service Messenger {
    rpc message(SendMessage) returns (Response);
    rpc receive(Receive) returns (stream ChatResponse);
    rpc login(LoginRequest) returns (Response);
    rpc group(GroupRequest) returns (GroupResponse);
}
```
```python
message GroupRequest {
    string userName = 1;
}
```
```python
message GroupResponse {
    string group = 1;
    string users = 2;
}
```
```python
message LoginRequest {
    string user = 1;
}
```
```python
message SendMessage {
    bytes message = 1;
    string sender = 2;
    string receiver = 3;
    google.protobuf.Timestamp timestamp = 4;
}
```
```python
message Response {
    string messages = 1;
//    google.protobuf.Timestamp timestamp = 2;
}
```
```python
message Receive {

    string sender = 2;
    string receiver = 3;
}
```
```python
message ChatResponse {
//    google.protobuf.Timestamp timestamp = 1;
    bytes messages = 2;
    string sender = 3;
//    string recipient = 4;
}
```
### Client Code
+ Get messages from server
+ Message decryption using AES from cryption library.
+ Load each response from LRU cache stored so messages are saved throughout sessions.
```python
def get_messages(stub, sender_name, receiver_name, key, iv):
    try:
        responses = stub.receive(SpartanMessenger_pb2.Receive(sender=sender_name, receiver=receiver_name))
        while (next(responses)):
            for request in responses:
                # Decryption
                decryption_suite = AES.new(key, AES.MODE_CFB, iv)
                plain_text_b = decryption_suite.decrypt(request.messages)
                plain_text = plain_text_b.decode('utf-8')
                # print('test')
                print('[' + request.sender + '] ' + plain_text)
        # print(responses)
    except Exception as error:
        print(str(error))
os._exit(1)
```

+ Open YAML to verify client login from list of groups.
```python
with open("config.yaml", 'r') as f:
    try:
        yamlconfig = yaml.load(f)
    except yaml.YAMLError as exc:
        print(exc)
        sender_name = sys.argv[1]
```
+ Login
```python
with grpc.insecure_channel('localhost:' + str(yamlconfig['port'])) as channel:
    stub = SpartanMessenger_pb2_grpc.MessengerStub(channel)
    loginAttempt = stub.login(SpartanMessenger_pb2.LoginRequest(user=sender_name))
    if loginAttempt.messages == 'unverified':
        print('[Spartan] User not found in config. Try rerunning.')
        sys.exit()
    elif loginAttempt.messages == 'verified':
      print('[Spartan] Connected to Spartan Server at port ' + str(yamlconfig['port']) + '.')
```
+ Verify if group is present and if it is display all users in group.
```python
#finding group group chat
groupresponse = stub.group(SpartanMessenger_pb2.GroupRequest(userName=sender_name))
if groupresponse.group == 'none':
    print('[Spartan] group not found...')
    sys.exit()
else:
    group = groupresponse.group
    userList = groupresponse.users
    print('[Spartan] Connected to ' + group + ' with users: ' + userList)
```
+ Security + threading
```python
#security AES
key = getGroupKey(group)
iv = getGroupIV(group)
threading.Thread(target=get_messages, args=(stub, sender_name, group, key, iv)).start()
```
+ Get input messages and allow for continuous sending over connection while checking for message exceed warning from server.
```python
while True:
    # messaging loop
    input_message = input()
    # Encryption
    encryption_suite = AES.new(key, AES.MODE_CFB, iv)
    cipher_text = encryption_suite.encrypt(input_message)
    response = stub.message(SpartanMessenger_pb2.SendMessage(message=cipher_text, sender=sender_name, receiver=group))
    if response.messages == 'Exceeded':
      print('Exceeded 3 message limit within 30s. Message not sent.')
```

### Server code
+ Group lists
```python
def group(self, request, context):
    with open("config.yaml", 'r') as f:
        try:
            yamlconfig = yaml.load(f)
        except yaml.YAMLError as exc:
            print(exc)
    for group in yamlconfig['groups']:
        if request.userName in yamlconfig['groups'][group]:
            userList = []
            for user in yamlconfig['groups'][group]:
                userList.append(user)
            users = str(userList)
            return SpartanMessenger_pb2.GroupResponse(group=group, users=users)
            return SpartanMessenger_pb2.GroupResponse(group='none', users='')
```
+ User login verification
```python

    def login(self, request, context):
        with open("config.yaml", 'r') as f:
            try:
                yamlconfig = yaml.load(f)
            except yaml.YAMLError as exc:
                print(exc)
        if request.user in yamlconfig['users']:
            return SpartanMessenger_pb2.Response(messages='verified')
        else:
          return SpartanMessenger_pb2.Response(messages='unverified')
```
+ Message Sending
```python

    def message(self, request, context):
        global LRU_cache
        #import config for LRU #
        with open("config.yaml", 'r') as f:
            try:
                yamlconfig = yaml.load(f)
            except yaml.YAMLError as exc:
                print(exc)
        #timestamps
        request.timestamp.GetCurrentTime()
        sTime = request.timestamp.ToSeconds()
        group = request.receiver
        #create new message for cache
        new_Message = create_message(request.sender, request.receiver, request.message, sTime)
        #check to see if time btwn msgs is too fast...
        if group in LRU_cache:
            if withinRateLimit(LRU_cache[group], new_Message):
                if LRU_cache_check(LRU_cache[group]):
                    LRU_cache[group].pop(0)
                    LRU_cache[group].append(new_Message)
                    return SpartanMessenger_pb2.Response(messages='Sent')
                else:
                    LRU_cache[group].append(new_Message)
                    return SpartanMessenger_pb2.Response(messages='Sent')
            else:
                return SpartanMessenger_pb2.Response(messages='Exceeded')
        else:
            temp_list = []
            temp_list.append(new_Message)
            LRU_cache[group] = temp_list
            return SpartanMessenger_pb2.Response(messages='Sent')
```
+ Message Receiving
```python
def receive(self, request, context):
    yield SpartanMessenger_pb2.ChatResponse(messages=b'accepted', sender = '')
    global LRU_cache
    with open("config.yaml", 'r') as f:
        try:
            yamlconfig = yaml.load(f)
        except yaml.YAMLError as exc:
            print(exc)
    group = request.receiver
    # if group doesnt exist create empty one
    if group not in LRU_cache:
        temp_list = []
        LRU_cache[group] = temp_list
    indexer = 0
    # retrieve old messages first by self as well
    while len(LRU_cache[group]) > indexer:
        unread_sender = LRU_cache[group][indexer]['sender']
        unread = LRU_cache[group][indexer]['message']
        old_msg = copy.deepcopy(LRU_cache[group])
        indexer += 1
        yield SpartanMessenger_pb2.ChatResponse(messages=unread, sender=unread_sender)
    #continously retrieve new messages only by other chatter
    max_cache = yamlconfig['max_num_messages_per_user']
    while True:
        while len(LRU_cache[group]) > indexer:
            unread_sender = LRU_cache[group][indexer]['sender']
            unread = LRU_cache[group][indexer]['message']
            old_msg = copy.deepcopy(LRU_cache[group])
            indexer += 1
            yield SpartanMessenger_pb2.ChatResponse(messages=unread, sender=unread_sender)
        if (len(LRU_cache[group]) == max_cache) and (old_msg != LRU_cache[group]):
            indexer = indexer -1
            unread_sender = LRU_cache[group][indexer]['sender']
            unread = LRU_cache[group][indexer]['message']
            old_msg = copy.deepcopy(LRU_cache[group])
            indexer += 1
            yield SpartanMessenger_pb2.ChatResponse(messages=unread, sender=unread_sender)
```
+ LRU Caching
```python
def lru_cache(func):
    def wrapper_lru(*args, **kwargs):
        with open("config.yaml", 'r') as f:
            try:
                yamlconfig = yaml.load(f)
            except yaml.YAMLError as exc:
                print(exc)
        kwargs['max_msg'] = yamlconfig['max_num_messages_per_user']
        return func(*args, **kwargs)
        return wrapper_lru
```
```python
@lru_cache
def LRU_cache_check(LRU_cache_group, *args, **kwargs):
    with open("config.yaml", 'r') as f:
        try:
            yamlconfig = yaml.load(f)
        except yaml.YAMLError as exc:
            print(exc)
    if len(LRU_cache_group) >= kwargs['max_msg']:
        return True
    else:
      return False
```
+ Message rate limiting
```python
def wrapper_rate(*args, **kwargs):
    with open("config.yaml", 'r') as f:
        try:
            yamlconfig = yaml.load(f)
        except yaml.YAMLError as exc:
            print(exc)
    kwargs['max_call'] = yamlconfig['max_call_per_30_seconds_per_user']
    return func(*args, **kwargs)
    return wrapper_rate
```
```python
def withinRateLimit(convolist, messagecheck, *args, **kwargs):
    count_Time = 0
    for message in convolist:
        if message['sender'] == messagecheck['sender']:
            duration = messagecheck['time'] - message['time']
            if duration <= 30:
                count_Time += 1
    if count_Time < kwargs['max_call']:
        return True
    else:
      return False
```
