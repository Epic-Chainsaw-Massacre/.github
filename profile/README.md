# Reverse Hangman Online

## Table of contents
   - [What is Reverse Hangman?](#what-is-reverse-hangman) <!-- probleem opstelling -->
     - [Rules](#rules)
     - [Single Round Explained](#single-round-explained)
     - [Points](#points)
   - [What is Reverse Hangman Online?](#what-is-reverse-hangman-online)
   - [Architecture](#architecture)

## What is Reverse Hangman
Reverse hangman is as it says, hangman but reversed. You try to guess out letters that the word does not contain. 

### Rules
The rules of this game are a bit more complicated than normal hangman so try to read it very carefully.

The game has 2 teams which both have a different role
- Wordmaster
- Guesser

The wordmaster starts the game by coming up with a word. This word will be only known by the wordmaster and not by the guesser. 

The word does have a few requirements
- The chosen word has to be an existing word on the website https://www.dictionary.com/
- the different letters of the word summed up has to be more than 3 and less than 9

If the word doesn't match one of those requirements the game should not start. If it does match the requirements, the game starts.

### Single round explained
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




### Points

## What is Reverse Hangman Online
What is Reverse Hangman Online

## Architecture
Architecture

<!-- this is a comment -->
