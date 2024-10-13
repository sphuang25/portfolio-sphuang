---
title: A4 Beta- Backend Design
layout: doc
---

# Assignment 4: Backend Design

## Main Updates

- 28 test cases written
> 28 Mocha test that represents some typical operations when interacting with this backend. It was not complete and did not cover all the edge cases. But it covered all the new operations in the app that I implemented, showing that the synchronization works!

- Using existing Friending, Posting Concept
> Comparing to A4-alpha, I have noticed Friending and Posting concepts are already included in the starter code. Hence, I will use the existing implementation and add adequate APIs when needed.

- Viewing Concept is removed
> Instead of a concept, I think it is more like a front end design choice. The interfacing concept along will have enough information for the front end to render.

- Separation of MessageDoc in Messaging Concept
> Usually, we think of a message including information of sender, receiver, and the content. Here I separated the messageDoc into the sender/receiver part and the content part, which makes better modularity. It is also easier to collect contents and do searching based on the keywords.

- Extra functions in Friending Concept
> I would like to design a more closed application, so only friends can see each others' post; hence, checking whether two users are friends is important.

## Concepts

### 1. Friending[User]

**Purpose:** 
> To grant access to other user to see oneself's activity and see others

**Principle:**
> If User a and User b are friend, the pair (a,b) or (b,a) will be stored in the friend database.

**State:**
> friends: set FriendshipDoc \
> user1: friends -> One User \
> user2: friends -> One User \
> requests: set FriendRequestDoc \
> from: requests -> One User \
> to: requests -> One User \
> status: requests -> "pending"|"rejected"|"accepted" \

**Actions:**

- getRequests(self: User)
- sendRequest(from: User, to: User) 
- acceptRequest(from: User, to: User)
- rejectRequest(from: User, to: User)
- removeRequest(from: User, to: User)
- removeFriend(user: User, friend: User)
- getFriends(user: User)
- isFriend(user1: User, user2: User)
- assertFriendsOrSelf(user1: User, user2: User)
- assertFriends(user1: User, user2: User)

### 2. Posting[User]

**Purpose:**
> To create content for various potential purposes.

**Principle:**
> A post content c will stored under the author u. (c, u) pairs will be stored.

**State:**
> posts: set PostDoc \
> author: posts -> One User \
> content: posts -> One String

**Actions:**

- create(author: User, content: string)
- getByAuthor(author: User)
- getAuthorOfPost(postID: PostDoc)
- update(_id: PostDoc, content?: string)
- delete(_id: PostDoc)
- assertAuthorIsUser(_id: PostDoc, user: User)

### 3. Authenticating

**Purpose:**
> Correspond the user of the app to real people.

**Principle**
> If one registers with a username and a password to make a user, one can access that user with a same pair.

**State**
> registeredUsers: set User \
> username: registeredUsers -> one String \
> password: registeredUsers -> one String 

**Actions**

- create(username: string, password: string)
- getUserById(_id: User)
- getUserByUsername(username: string)
- idsToUsernames(ids: Array[User])
- getUsers(username: string)
- authenticate(username: string, password: string)
- updateUsername(_id: User, username: string) 
- updatePassword(_id: User, currentPassword: string, newPassword: string) 
- delete(_id: User)
- assertUserExists(_id: User)

### 4. Sessioning[User]

**Purpose:**
> Extend authenticated actions for the user that has previously authenticated.

**Principle**
> After a session starts, getUser action returns the user identified in the same session.

**State**
> active: set Session \
> user: active -> one User

**Actions**
- start(session: Session, user: ObjectId)
- end(session: Session)
- getUser(session: Session)
- isLoggedIn(session: Session)
- isLoggedOut(session: Session)

### 5. Interfaceing[User]

**Purpose**
> Serve as different "modes" in the application.

**Principle**
> If a user u has an interface i, (u, i) pair is stored.

**State**
> interfaces: set InterfaceDoc \
> user: interfaces -> One User \
> interface: interfaces -> interfaceType \ 
> enum interfaceType = {"Focus", "Leisure"}

**Actions**
- createInterface(userID: User, interfaceForUser: interfaceType)
- getInterfaceEnumBystring(interfaceString: string)
- switchInterface(userID: User, interfaceForUser: interfaceType)
- getInterface(userID: User)
- getInterfaceOwner(interfaceID: interfaceDoc)

### 6. Labelling[Item]
**Purpose**
> Let the item creator record keywords or information of items. 

**Principle**
> A label l is attached with the item i when being labelled. i can be associate with multiple ls. (i, [l1,l2,...]) is stored.

**State**
> labels: set Label
> item: labels -> one Item
> label: labels -> set string

**Actions**
- getLabelsOfItem(item: Item)
- getLabelDoc(item: Item)
- appendLabel(item: Item, content: string) 
- removeLabelByIndex(item: Item, index: string)
- removeLabelByContent(item: Item, content: string)
- searchLabel(itemsToSearch: Array[Item], label: string) 

### 7. Messaging[User, Item]
**Purpose**
> Communicate with other users by sending message directly to another user.

**Principle**
> An item i and a content c forms a message m. (i, c) is stored \
> If m is sent from a sender s to receiver r, (s, r, m) is stored. \
> For each triplet (s, r, m), s and r owns the message.

**State**
> messageRecords: set MessageRecord \
> messages: set Message \
> sender: messageRecords -> one User \
> receiver: messageRecords -> one User \
> message: messageRecords -> one Message \
> item: messages -> One Item \
> words: messages -> One string

**Actions**
- createMessage(item: Item, words: string)
- getMessageItem(messageID: Message)
- sendMessage(sender: User, receiver: User, message: Message)
- deleteMessage(messageID: Message)
- getMessageRecord(messageID: Message)
- getMessageSender(messageID: Message)
- getMessageReceiver(messageID: Message)
- getAllMessageRecordUser(userID: User)
- getAllMessageUser(userID: User)
- getAllItemUser(userID: User)
- getAllSentMessages(userID: User)
- ownsMessage(userID: User, messageID: Message)

## Applications
App Grasp
- getSessionUser(session: Session)
> Sessioning, Authenticating

- getUsers()
> Authenticating

- getUser(username: string)
> Authenticating

- createUser(session: Session, username: string, password: string)
> Sessioning, Authenticating, Interfacing

- updateUsername(session: Session, username: string)
> Sessioning, Authenticating

- updatePassword(session: Session, currentPassword: string, newPassword: string) 
> Sessioning, Authenticating

- deleteUser(session: Session)
> Sessioning, Authenticating

- login(session: Session)
> Sessioning, Authenticating

- logout(session: Session)
> Sessioning, Authenticating

- getPosts(author?: string) 
> Authenticating, Posting

- createPost(session: Session, content: string)
> Sessioning, Posting

- updatePost(session: Session, id: Item, content: string)
> Sessioning, Posting

- deletePost(session: Session, id: Item)
> Sessioning, Posting

- getFriends(session: Session)
> Sessioning, Authenticating, Friending

- removeFriends(session: Session, friend: string)
> Sessioning, Authenticating, Friending

- getRequests(session: Session)
> Sessioning, Friending

- sendFriendRequest(session: Session, to: string)
> Sessioning, Authenticating, Friending

- removeFriendRequest(session: Session, to: string) 
> Sessioning, Authenticating, Friending

- acceptFriendRequest(session: Session, from: string) 
> Sessioning, Authenticating, Friending

- rejectFriendRequest(session: Session, from: string) 
> Sessioning, Authenticating, Friending

- addLabel(session: Session, id: Item, content: string)
> Sessioning, Posting, Labelling

- checkLabel(session: Session, id: string)
> Sessioning, Posting, Labelling

- deleteLabelByIndex(session: Session, id: string, labelIdx: string)
> Sessioning, Posting, Labelling

- deleteLabelByContent(session: Session, id: string, content: string)
> Sessioning, Posting, Labelling

- checkInterface(session: Session, usernameToCheck: string)
> Sessioning, Authentiating, Friending, Interfacing

- setInterface(session: Session, interfaceType: string)
> Sessioning, Interfacing

- poke(session: Session, username: string, messageString?: string)
> Sessioning, Authentiating, Friending, Interfacing, Messaging

- getAllMessages(session: Session) 
> Sessioning, Messaging

- sendMessage(session: Session, receiverName: string, messageItemID: string, messageContent: string)
> Sessioning, Authentiating, Friending, Messaging

- searchMessageWithLabel(session: Session, label: string) 
> Sessioning, Messaging, Labelling

- deleteMessage(session: Session, messageIDStr: string)
> Sessioning, Messaging

## Diagram of the States.

The states of the app can be represented as the following diagram, which is same as A4 alpha.
<img src="./A4_diagram.png" width="600" height="400">

## Design Choice

1. No public posts/ Limitation of Message

> There are always some sort of public concept in current applications, where of course, maximize the potential to interact with more people. Yet, I have been discovering some posts on Threads that they was initially targetting their followers but got exposed due to the algorithm. Some harms were made. You can set to private mode in Threads, but I would prefer to have everything private to avoid this. One specific example in my implementation is that, if you were to send a post to your friends, that specific post must be visible to both the sender and the receiver, i.e. both people should be friend with the author of the post.

2. Labelling Searching

> My initial idea is to treat message and post the same (as an Item). However, it bothered me when I was implementing label. If the message can be shared by another message, then can I label the message itself? Or should I limit the label to the original post that was first shared in the nested sharing message? Due to these complexity, I limited labelling on post, and the item that is included in the message must be a post.

3. Who should set the label?

> One of my objective is to let user quickly obtain a specific message or post they comes to mind by searching with labels. Hence, intuitively, each user can customize their own labels on every post. However, something feels wrong to me about people adding "captions" on their own to each content. Also, the storage of these labels can be large. Hence, I limited the labelling access to the original post author, which I think this will also lead to some problems, for example, typing certain illegal keywords that attracts people to the author's profile. But I think it will be clearer when I get to the next phase.