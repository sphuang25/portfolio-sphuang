---
title: A5α - Frontend Design
layout: doc
---

# Assignment 5: Frontend Design

## Heuristic Evaluation and Writeup

### Usability Criteria

- Safety: how does the interface guard against people making mistakes?
> In my focus mode, where each action is as simple as a search bar and click "submit", but each actions comes with a selection bar. For example, if you were to poke someone, you will be able to see a list of who you can poke, instead of typing in a wrong username by accident. A stricter way of preventing mistake is to add a confirm message before the action takes effect, but I feel like this additional click is somehow annoying to the user.

- Security: how does the interface ensure a user’s privacy and integrity?
> I think this is a key design of my backend, where some functions are implemented that only the posts are only accessible by author's friends. Also, if you were to share/referencing a content to your friend, that content should be both visible by the sender and the receiver. As I mentioned in A4, I have seen a post about a matter that was private, important to the author but not crucial to the society. It was intended to express his feeling to his friends, but somehow got favored by the algorithm. Yet, it was Here I made this application more private, and you can think of it as the Instagram's close friends functionality.

### Physical Heuristics

- Gestalt principles: does the layout of the interface elements convey conceptual structure?
> I would like to bring up my interface concept. In the friend list, you can see the list of friends with their corresponding interface, which gives a direct picture that each user correspond to a single interface. For the labelling concept, since the label editting icon is besides every post, it gives the user a picture that each item is associate with label; the only concern I have here is that, here I only allow the author to edit the label, which might be confusing for the user when they want to add a label to every post. I would probably modify the backend implementation in this case.

- Situational context: how does the interface convey to a user their context (where they are, the app’s state, etc.), and how does it adapt to their context?
> In my focus mode, the search bar should update you select the action. For example, you first select what to do; once you pick the "message" option, another box saying "select user" appears. This gives user a clear image on which state they are on now. Also in the leisure node, I think a small indication on the navigation bar on the right with post, friends, messages,... can be added, so that the user knows which functionality they are on right now.

### Linguistic Level

- Speak a user’s language: does the interface use simple, helpful informative messages? are there instances where messages might only be understandable by developers?
> In both the focus and leisure mode, there are always hint messages in the text box, for example, "type something here...", or "I would like to...", which were inspired by the search bar of Instagram, which I think to be clear for the user. The way I designed for my focus mode, which might be unfamiliar to new users at first, since the interface is a lot different from the existing social media, but I think with the hint message, it should be clear and easy to get familiar with.

- Information scent: how does the interface provide hints for navigation to aid a user in “foraging” for information?
> The leisure mode is as simple as a main scrolling area and a tab on the right. I think this enables user to click on different tab to try on different things. As for the focus mode, the only operation is to select from the selection bar. However, I think it is indeed a little unclear for the first-time user to operation it. I will try to add a question mark icon and provide information from there.

