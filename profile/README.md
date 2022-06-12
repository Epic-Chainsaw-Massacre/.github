# Reverse Hangman Online

![reverseHangmanHeadReverse2](https://user-images.githubusercontent.com/74303221/171628758-69143445-85e8-4de8-aa55-1c4c3a18258a.png)

## Table of contents
   - [What is Reverse Hangman?](#what-is-reverse-hangman) <!-- probleem opstelling -->
     - [Rules](#rules)
     - [Single Round Explained](#single-round-explained)
     - [Tiebreaker](#tiebreaker)
     - [Points](#points)
     - [Strategy](#strategy)
   - [What is Reverse Hangman Online?](#what-is-reverse-hangman-online)
     - [Differences with the original](#differences-with-the-original)
   - [Architecture](#architecture)
     - [Structure](#structure)
     - [Coding Languages](#coding-languages)
     - [Database](#database) 
     - [C-Models](#c-models)
   - [Project Management](#project-management)
     - [Project Idea](#project-idea)
     - [User Stories](#user-stories)   

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
The guesser simply guesses letters trying to avoid the ones that are in the word. Ff the goal is reached while guessing, the guesser can decide to continue playing for double or nothing. If he does continue, the guesser has to guess out all letters that aren't in the word. If he loses all his lives in this process he gets 0 points, if he succeeds, his points are doubled.

## Single round explained
I will try to explain a game of Reverse Hangman using images from an application I made earlier [ReverseHangmanDesktop](https://github.com/CrossyChainsaw/ReverseHangmanDesktop). I will first explain something and below the explanation I will put an image that represents the stuff I explained.

So, the game starts with the wordmaster choosing a word

![image](https://user-images.githubusercontent.com/74303221/171599834-2db01975-7159-4286-a4b7-fc0ce9ff9243.png)

The wordmaster chooses the word 'Clock'. The word Clock has 5 letters in total and 4 different letters. to calculate the goal and lives we use formulas. These formulas have originated by just playing the game slot with different formulas and this felt the fairest for both teams.
- Lives = [Math.Floor](https://docs.microsoft.com/en-us/dotnet/api/system.math.floor?view=net-6.0)(different letters / 2) + 1 = [Math.Floor](https://docs.microsoft.com/en-us/dotnet/api/system.math.floor?view=net-6.0)(4 / 2) + 1 = 5
- Goal < different letters * 2 = 4 * 2 = 8

![image](https://user-images.githubusercontent.com/74303221/171601772-d4875de7-607b-4bb5-89dd-2ddf02c61069.png)

From now of on the Guesser will start guessing. Let's say the Guesser guesses the letter 'Z'. The letter Z is not in the word. The result after the guess is that remaining letters now is 25. Lives aren't affected at all since the letters isn't in the word.

![image](https://user-images.githubusercontent.com/74303221/171605450-fb7dd974-2e2b-477c-91f7-69001719bbf4.png)

Now the Guesser guesses the letter 'O'. The O is in the word, this means the O gets displayed in the word, this also means remaining letters is one less, also does it mean you lost a life.

![image](https://user-images.githubusercontent.com/74303221/171604631-bed6db7b-0c0c-4069-b324-e0479f6cd470.png)

Let's guess a lot of letters that aren't in the word. Here we guessed away the following letters: A, B, D, E, F, G, H, I, J, M, N, W, X, Y. Since none of these are in the word, lives remain the same. remaining letters becomes 10 now.

![image](https://user-images.githubusercontent.com/74303221/171618771-1e4a1a23-f9ea-4dfc-9816-a6e9ecc42518.png)

We guessed the letter 'C', the letter C is twice in the word. This makes no difference. result is -1 life and -1 remaining letters.

![image](https://user-images.githubusercontent.com/74303221/171620117-e75019fb-5201-4836-a179-59be767769ae.png)

So now we have 1 life left. and 9 remaining letters. This means if we guess a letter that's in the word we lose this round. Let's guess the letter 'V' and 'T'. The letter V and T both aren't in the word so we reached our goal, <8, with lives remaining. This means we get 1 point, and a decision. We can wager the points we won this round and double them if we guess out all letters to double our points. looking at the image I can't figure out what the word might be, so to play it safe I stop here and take my one point.

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
- Failed Clean Sweep (+0 points): you continued after a Sweep but lost all lives in the process.
- Noble Sweep (+3 points): you reached the goal without losing a single life.
- Clean Noble Sweep (+6 points): you reached the goal without losing a life, continued, guessed away all letters that aren't in the word, but lost at least a single life in the process
- Royal Sweep (+10 points): guessed away all letters that aren't in the word without losing a single life.

## Strategy
In the end of the day, if you understand all rules, it's quite a simple game. To add more excitement to your games I'll share some strategies we found out playing this game.

### Trade life for clean sweep

#### Example 1 (Free Clean Sweep)
Let's look at the picture below. A few important things to see here. First of all, we have 2 lives and we have to guess only one letter to reach the goal. In this case the only words it can be are 'Hazy' and 'Lazy'. Guessing an 'A' or a 'Y' would be completely irrelevant. If we guess the letter 'L' or 'H' we filter out one of the words leaving only the other word. and since you have 2 lives this is a safe bet. 

![image](https://user-images.githubusercontent.com/74303221/171993891-e599740f-d323-4287-85b0-1f2d7a7b3ae5.png)

Let's say we guess the letter 'H'. Letter 'H' isn't in the word, so the word has to be 'Lazy'.

![image](https://user-images.githubusercontent.com/74303221/171994066-3c15008e-43ef-43d1-86f0-978cf178005a.png)

This gives us 2 points instead of 1. watch out for these free opportunities.

![image](https://user-images.githubusercontent.com/74303221/171994164-c11cfd23-a799-435a-a927-554fea0cc2b4.png)

#### Example 2 (Bigger chance to Clean Sweep)
In this example I will keep the word secret for now ;). In this example we already guessed the letter 'A'. we have 3 lives left, and we can guess 2 more letters. This means that if we guess wrong twice, we still have a life remaining. If we look at the word now we can't guess what it is. An, in my opinion, good strategy here would be to intentionally do a wrong guess twice here. We should try to guess letters that are uncommon but fit in the word (You can also start guessing wrong when you have to guess 3 letters or 4 letters but it adds a little risk to it if you only have 3 lives, since you might just guess wrong 3 times and lose all lives without getting a single point).

![image](https://user-images.githubusercontent.com/74303221/171994781-1594f439-50ce-4528-8ce5-7bd2d160146a.png)

For example we guess the letters 'T' and 'R'.

![image](https://user-images.githubusercontent.com/74303221/171995093-4ea15a0f-2f58-4b02-ad34-a9b4be42747c.png)

I guess the word is 'Starfish', so hope for the best.

![image](https://user-images.githubusercontent.com/74303221/171995137-82e3cc06-ef91-482b-8cef-a85e5d843d3b.png)

And surprise surprise. Clean sweep.

### No Q no U
If a word doesn't have a 'Q' the chance of having an 'U' is very low. The words that have a 'Q' but no 'U' are usually imported form different languages. There are also more combinations like this, for example a 'C' gets along with 'U' very well. Important to note is that this isn't always the case.

### The Obvious Letters
If you are the wordmaster, try avoid making words with an 'X', 'Z', 'J'. These letters are the least used in the English language. This means that if you make a word with these letters, it's easier for the guesser to filter out words. Try making words with the most common letters in the alphabet, so that of the guesser has 1 life left, the words isn't obvious, so you avoid giving out Clean sweeps.


# What is Reverse Hangman Online
Reverse Hangman Online is a web application where you can play Reverse Hangman. At the end of my semester I want to have a website everyone can visit to play reverse hangman. 

## Differences with the original
Here's a list of stuff that will be different
- Its digital, the original Reverse Hangman was on a whiteboard or on a piece of paper. i've made a small application that represents it, but the original was just old school on a whiteboard.
- it's online, the end goal is to have 2 clients be able to play against each other. 
- Ranking system, since people can play online against each other, it would also be really fun if you can match the good players with the good, and the bad players with the bad. this makes it that people play against others of their own level. this is usually more enjoyable.
- Tiebreaker, since everything will be online and digital, there is no option to Rock Paper Scissor in real life. that's why I want to make an in-game tiebreaker with a selection of minigames. I'm sticking with a minigame to decide the tiebreaker since that's a really fun functionality in my opinion.
- Game History, data of a completed game will be saved to a database. with this data we can do fun stuff for example show what words get used a lot. which letters get guessed first. stuff like that.

# Architecture
In this section you can find all my technical decisions, and why I chose them.

## Structure
This semester I make use of an microservice structure. I choose this structure since I thought it was a must to choose microservices for your individual project. Other than that there isn't another reason I choose a microservice structure. Later on, in the final 3 weeks, I found out it wasn't a must to choose a microservice architecture. If i found out earlier I might have chosen a different structure since it's quite a mess to maintain like 4 different applications by yourself for a small school project. But in the end of the day it was a good practise so it wasn't a waste of time.

## Coding Languages
### Java
This semester we have to make use of an OO based language. We got recommended to learn a new OO language, Java in particular. I thought it would be a pretty good idea to learn a new language so I picked up Java.

### C#
When I started to understand microservices I also understood it would be possible to make use of multiple coding languages. When I found out about this I instantly wanted to also make use of C# in my project, since:
- It looked interesting to me to make 2 different applications with different coding languages talk to each other
- I already have lots of experience in C#
- I find coding in C# more fun than in java
- I already coded all the logic behind Reverse Hangman in an application called [ReverseHangmanDesktop](https://github.com/CrossyChainsaw/ReverseHangmanDesktop) in C#. this means I can copy paste all logic from the other application I made, and this saves lots of time.

### React
in the requirements of the semester it's listed that we have to make use of an JavaScript front-end framework. In our group project we decided to choose React as the framework since one group member already had lots of experience in react. so if we ever got stuck with something he could always help us. Since we were using React in our group project it would be a pretty logical decision to also use it in my individual project, since it would take a lot of time to learn 2 different front-end frameworks at the same time, especially if you never learned one before.

### Typescript
In the requirements of the semester it's stated that we have to make use of JavaScript. I heard a lot of classmates chatting about TypeScript. It appears to be a language that translates itself to JavaScript. The main reason why I chose TypeScript over JavaScript, is because in the IDE it shows errors while trying to build. If there is an error the application won't start. This makes it so much easier to find errors in my application. And in the end that saves a lot of time.

## Database
**(Not-Researched-Yet)**
I've chosen for an MYSQL database, this was the result of a small research for the GP I did. BUT the database I use in my IP can still change. I want to do a good research to find out which database fits my application the best. 
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.
EXPLAIN WHAT DATABASE I USE AND WHY.

## C-Models
You are going to see some images of my C-Models. These images were originally pretty big. I tried to make them a bit smaller but they are still huge. If i made them even smaller the quality dropped too much. I'd advise zooming your screen out or just clicking the image. This makes it easier to read and view the image at the same time.

### C1-Model
I visualised my architecture with c-models. here is my C1-Model. There isn't much to see. You see a client visit the game, the game makes use of an [external dictionary API](https://dictionaryapi.com/). More information in the C2-Model.

![image](https://user-images.githubusercontent.com/74303221/173081888-a9fdb663-8cb3-48ec-a051-19795778c7fa.png)


### C2-Model
So here you see 4 applications. Lets slowly go through the flow of my application. The client visits the [front-end](https://github.com/Epic-Chainsaw-Massacre/reverse-hangman-online-frontend). The client fills in a word, this word gets send to the [WordService](https://github.com/Epic-Chainsaw-Massacre/Word-Service). WordService makes use of the external dictionary API to check if the word exists. The WordService sends the result of this check back to the frontend. If the result says the word exists, the game starts. This is where the [Backend](https://github.com/Epic-Chainsaw-Massacre/reverse-hangman-online-backend) comes into play. All logic of the game itself is coded in the backend. the front-end and the backend talk using JSON. the front-end asks for something and the backend gives it. if the game is completed, the [GameHistoryService](https://github.com/Epic-Chainsaw-Massacre/Game-Statistics-Service) comes in action. The front-end sends the game results to the GameHistoryService. The GameHistoryService saves these results to a MYSQL database. In the future it would be a cool idea to do something with this data. For example show what letters get guessed the most in the first turn or what words get used the most.

![image](https://user-images.githubusercontent.com/74303221/173081971-f686f2f8-303d-4d42-bb5f-d2fdd5464ac1.png)

# Project Management
The project is being managed on the GitHub organization called [Reverse Hangman Online](https://github.com/Epic-Chainsaw-Massacre). If you just clicked the link or checked my repositories you can see its sometimes called 'Epic Chainsaw Massacre'. This is an outdated name, it refers to Reverse Hangman Online.

## Project Idea
For my individual project I wanted to make a game. In general I always enjoy coding games, for me it keeps school pretty fun. I made a prototype-ish application in WinForms called [ReverseHangmanDesktop](https://github.com/CrossyChainsaw/ReverseHangmanDesktop). The game was original so I really wanted to take it more seriously. That's why I decided to choose it for my individual project

## User Stories
I started making requirements for my application using my own wishes. Everything I'd like to see in the game became a requirement. I rewrote the requirements as user stories. You can find all my user stories back in my [Organizations' project](https://github.com/orgs/Epic-Chainsaw-Massacre/projects/2). Here I can assign an user story to 'Todo', 'In Progress' and 'Done'. Also you see labels. I added labels to each user story to show which application it belongs to. Now why did I use labels you'd ask? It's because of a missing important functionality in GitHub. It is not possible to link a user story to 2 different repositories, strange I know. I tried to work my way arround this problem by putting all user stories in a single repository. All user stories are located in [Reverse-Hangman-Online-Frontend](https://github.com/Epic-Chainsaw-Massacre/reverse-hangman-online-frontend), in the issues section. This is the reason why I use labels to indicate what application an user story belongs to.

<!-- this is a comment -->
