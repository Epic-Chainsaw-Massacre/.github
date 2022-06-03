# Reverse Hangman Online

![reverseHangmanHeadReverse2](https://user-images.githubusercontent.com/74303221/171628758-69143445-85e8-4de8-aa55-1c4c3a18258a.png)

## Table of contents
   - [What is Reverse Hangman?](#what-is-reverse-hangman) <!-- probleem opstelling -->
     - [Rules](#rules)
     - [Single Round Explained](#single-round-explained)
     - [Tiebreaker](#tiebreaker)
     - [Points](#points)
   - [What is Reverse Hangman Online?](#what-is-reverse-hangman-online)
     - [Differences with the original](#differences-with-the-original)
   - [Architecture](#architecture)
     - [Structure](#structure)
     - [Coding Languages](#coding-languages)
     - [Database](#database) 
     - [C-Models](#c-models)
   - [Project Managment](#project-managment)

# What is Reverse Hangman
Reverse hangman is as it says, hangman but reversed. You try to guess out letters that the word does not contain. 

## Rules
The rules of this game are a bit more complicated than normal hangman so try to read it very carefully.

The game has 2 teams which both have a different role
### Wordmaster

The wordmaster starts the game by coming up with a word. This word will be only known by the wordmaster and not by the guesser. 

The word does have a few requirements
- The chosen word has to be an existing word on the website https://www.dictionary.com/
- the different letters of the word summed up has to be more than 3 and less than 9

If the word doesn't match one of those requirements the game should not start. If it does match the requirements, the game starts.

### Guesser
The guesser simply guesses letters trying to avoid the ones that are in the word. Ff the goal is reached while guessing, the guesser can decide to continue playing for double or nothing. If he does continue, the guesser has to guess out all letters that aren't in the word. If he loses all his lives in this proces he gets 0 points, if he succeeds, his points are doubled.

## Single round explained
I will try to explain a game of Reverse Hangman using images from an appllication I made earlier [ReverseHangmanDesktop](https://github.com/CrossyChainsaw/ReverseHangmanDesktop). I will first explain something and below the explanation I will put an image that represents the stuff I explained.

So the game starts with the wordmaster choosing a word

![image](https://user-images.githubusercontent.com/74303221/171599834-2db01975-7159-4286-a4b7-fc0ce9ff9243.png)

The wordmaster chooses the word 'Clock'. The word Clock has 5 letters in total and 4 different letters. to calculate the goal and lives we use formulas. These formulas have orginated by just playing the game slot with different formulas and this felt the most fair for both teams.
- Lives = [Math.Floor](https://docs.microsoft.com/en-us/dotnet/api/system.math.floor?view=net-6.0)(different letters / 2) + 1 = [Math.Floor](https://docs.microsoft.com/en-us/dotnet/api/system.math.floor?view=net-6.0)(4 / 2) + 1 = 5
- Goal < different letters * 2 = 4 * 2 = 8

![image](https://user-images.githubusercontent.com/74303221/171601772-d4875de7-607b-4bb5-89dd-2ddf02c61069.png)

From now of on the Guesser will start guessing. Lets say the Guesser guesses the letter 'Z'. The letter Z is not in the word. The result after the guess is that remaining letters now is 25. Lives aren't affected at all since the letters isn't in the word.

![image](https://user-images.githubusercontent.com/74303221/171605450-fb7dd974-2e2b-477c-91f7-69001719bbf4.png)

Now the Guesser guesses the letter 'O'. The O is in the word, this means the O gets displayed in the word, this also means remaining letters is one less, also does it mean you lost a life.

![image](https://user-images.githubusercontent.com/74303221/171604631-bed6db7b-0c0c-4069-b324-e0479f6cd470.png)

Lets guess alot of letters that aren't in the word. Here we guessed away the following letters: A, B, D, E, F, G, H, I, J, M, N, W, X, Y. Since none of these are in the word, lives remain the same. remaining letters becomes 10 now.

![image](https://user-images.githubusercontent.com/74303221/171618771-1e4a1a23-f9ea-4dfc-9816-a6e9ecc42518.png)

We guessed the letter 'C', the letter C is twice in the word. This make no difference. result is -1 life and -1 remaining letters.

![image](https://user-images.githubusercontent.com/74303221/171620117-e75019fb-5201-4836-a179-59be767769ae.png)

So now we have 1 life left. and 9 remaining letters. This means if we guess a letter that's in the word we lose this round. lets guess the letter 'V' and 'T'. The letter V and T both aren't in the word so we reached our goal, <8, with lives remaining. This means we get 1 point, and a decision. We can wager the points we won this round and double them if we guess out all letters to double our points. looking at the image i cant figure out what the word might be, so to play it safe i stop here and take my one point.

![image](https://user-images.githubusercontent.com/74303221/171620477-b90961df-9af0-47a0-94b2-fdc6aa9355bd.png)

If we would've decided to continue and correctly guess away the letters, we would've doubled the single point we got so we get 2 points.

![image](https://user-images.githubusercontent.com/74303221/171633133-606000ef-b056-4ee8-8a29-cbaf610937c7.png)

## Tiebreaker
If you read it all very carefully, you might have noticed something. The guesser can run out of lives and reach the goal in the same guess. If this happens, the two teams both choose a member to make them play the tiebreaker. The tiebreaker consists of a minigame you guys can choose, usually it's Rock Paper Scissors. But the people who are free to decide what the tiebreaker will be.

<!-- Ergens staat een fout over goal < 2 -->

## Points
First of all, only the guesser can earn points. The guesser can win a few different amount of points. Here are all of them explained
- Sweep (+1 point): reached the goal and lost at least one life.
- Clean Sweep (+2 points): guessed out all letters that aren't in the word, and has at least a life remaining.
- Failed Clean Sweep (+0 points): you continued after a Sweep but lost all lives in the proces.
- Noble Sweep (+3 points): you reached the goal without losing a single life.
- Clean Noble Sweep (+6 points): you reached the goal without losing a life, continued, guessed away all letters that aren't in the word, but lost at least a single life in the proces
- Royal Sweep (+10 points): guessed away all letters that aren't in the word without losing a single life.

# What is Reverse Hangman Online
Reverse Hangman Online is a web application where you can play Reverse Hangman. At the end of my semester i want to have a website everyone can go to to play reverse hangman. 

## Differences with the original
Here's a list of stuff that will be different
- Its digital, the original Reverse Hangman was on a whiteboard or on a piece of paper. i've made a small application that represents it, but the original was just old school on a whiteboard.
- it's online, the end goal is to have 2 clients be able to play against each other. 
- Ranking system, since people can play online against each other, it would also be really fun if you can match the good players with the good, and the bad players with the bad. this makes it that people play against others of their own level. this is usually more enjoyable.
- Tiebreaker, since everything will be online and digital, there is no option to Rock Paper Scissor in real life. that's why I want to make an ingame tiebreaker with a selection of minigames. I'm sticking with a minigame to decide the tiebreaker since thats a really fun functionality in my opinion.
- Game History, data of a completed game will be saved to a database. with this data we can do fun stuff for example show what words get used alot. which letters get guessed first. stuff like that.

# Architecture
In this section you can find all my technical decisions, and why i chose them.

## Structure
This semester I make use of an microservice structure. I choose this structure since i thought it was a must to choose microservices for your individual project. Other than that there isn't another reason i choose a microservice structure. Later on, in the final 3 weeks, i found out it wasn't a must to choose a microservice architecture. If i found out earlier i might have chosen a different structure since it's quite a mess to maintain like 4 different applications by yourself for a small school project. But in the end of the day it was a good practise so it wasn't a waste of time.

## Coding Languages
### Java
This semester we have to make use of an OO based language. We got recommended to learn a new OO language, Java in particular. I thought it would be a pretty good idea to learn a new language so i picked up Java.

### C#
When i started to understand microservices i also understood it would be possible to make use of multiple coding languages. When i found out about this i instantly wanted to also make use of C# in my project, since:
- It looked interesting to me to make 2 different applications with different coding languages talk to each other
- I already have lots of experience in C#
- I find coding in C# more fun than in java
- I already coded all the logic behind Reverse Hangman in an application called [ReverseHangmanDesktop](https://github.com/CrossyChainsaw/ReverseHangmanDesktop) in C#. this means i can copy paste all logic from the other application I made, and this saves lots of time.

### React
in the requirements of the semester it's listed that we have to make use of an JavaScript front-end framework. In our group project we decided to choose React as the framework since one group member already had lots of experience in react. so if we ever got stuck with something he could always help us. Since we were using React in our group project it would be a pretty logical decision to also use it in my individual project, since it would take alot of time to learn 2 different front-end frameworks at the same time, ecspecially if you never learned one before.

### Typescript
In the requirements of the semester it's stated that we have to make use of JavaScript. I heared alot of classmates chatting about TypeScript. It appears to be a language that translates itself to JavaScript. The main reason why i chose TypeScript over JavaScript, is because in the IDE it shows errors while trying to build. If there is an error the application won't start. This makes it so much easier to find errors in my application. And in the end that saves alot of time.

## Database
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.

## C-Models
You are going to see some images of my C-Models. These images were originally pretty big. I tried to make them a bit smaller but they are still huge. If i made them even smaller the quality dropped too much. I'd advise zooming your screen out or just clicking the image. This makes it easier to read and view the image at the same time.

### C1-Model
I visualised my architecture with c-models. here is my C1-Model. There isn't much to see. You see a client visit the game, the game makes use of an [external dictionary API](https://dictionaryapi.com/). More information in the C2-Model.

![image](https://user-images.githubusercontent.com/74303221/171859622-b3d924e8-3ab6-4da5-ab1c-427d2d1d5575.png)

### C2-Model
So here you see 4 applications. Lets slowly go through the flow of my application. The client visits the [front-end](https://github.com/Epic-Chainsaw-Massacre/reverse-hangman-online-frontend). The client fills in a word, this word gets send to the [WordService](https://github.com/Epic-Chainsaw-Massacre/Word-Service). WordService makes use of the external dictionary API to check if the word exists. The WordService sends the result of this check back to the frontend. If the result says the word exists, the game starts. This is where the [Backend](https://github.com/Epic-Chainsaw-Massacre/reverse-hangman-online-backend) comes into play. All logic of the game itself is coded in the backend. the front-end and the backend talk using JSON. the front-end asks for something and the backend gives it. if the game is completed, the [GameHistoryService](https://github.com/Epic-Chainsaw-Massacre/Game-Statistics-Service) comes in action. The front-end sends the game results to the GameHistoryService. The GameHistoryService saves these results to a MYSQL database. In the future it would be a cool idea to do something with this data. For example show what letters get guessed the most in the first turn or what words get used the most.

![image](https://user-images.githubusercontent.com/74303221/171856594-17ae166a-8116-44a8-9657-5ae43cb20f8e.png)


# Project Managment
The project is being managed on the GitHub organization called [Reverse Hangman Online](https://github.com/Epic-Chainsaw-Massacre). If you just clicked the link or checked my repositories you can see its sometimes called 'Epic Chainsaw Massacre'. This is an outdated name, it refers to Reverse Hangman Online.

## Problem
(Het volgende wat je gaat lezen is best wel stom en geforceerd, kan ik dit anders aanpakken?)
I started the project by creating a problem. My problem:

I am a gamer that gets bored when i have a small amount of free time. For example, breaks at school. They are only 1 hour long and usually we just hang arround being bored. In an hour you have enough time to play any game, but another issue is that the internet connection at school isn't ideal. I am a big fan of word games, ecspecially hangman. But after playing hours of hangman this started to bore me. I'd like to play a game that looks like hangman but with a twist. I want the game to be challenging so i won't get bored, also implementing an online multiplayer would awesome.

## User Stories
Using this problem i started to formulate requirements for my application. I rewrote the requirements as user stories. You can find all my user stories back in my [Organizaitons' project](https://github.com/orgs/Epic-Chainsaw-Massacre/projects/2). Here i can assign an user story to 'Todo', 'In Progress' and 'Done'. Also you see labels. I added labels to each user story to show..................... missing functionality github.

NOG NIET AF HIER

<!-- this is a comment -->
