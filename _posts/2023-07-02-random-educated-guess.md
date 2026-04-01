---
title: "A random educated guess"
date: 2023-07-02
author: Mauro Ghiani
category: Odds
tags: [Mathematics, Probability, OptimalStopping, SecretaryProblem, FSharp]
excerpt: "Optimal stopping is the art of making informed decisions based on accumulated knowledge when faced with random events."
---

Boris **Berezovsky**, born in Moscow in 1946 and passing away in Ascot, England in 2013, was a prominent Russian businessman, oligarch, government official, engineer, and mathematician.

He piled his wealth during the 1990s, capitalizing on the privatization of state assets in Russia. His holdings included significant control over Channel One, the country's main television channel.

Furthermore, Berezovsky was an advisor to Russian President Boris Yeltsin, but his relationship with Yeltsin's successor, Vladimir Putin, soured, leading him to flee Russia in 2000.

Berezovsky's academic background in **mathematics** played a vital role in his successful business enterprises.
Graduating with a degree in applied mathematics from the Moscow Forestry Engineering Institute in 1968, he later earned a doctorate in technical sciences from the Institute of Control Sciences of the USSR Academy of Sciences in 1983.

His mathematical prowess proved invaluable, enabling him to make well-informed decisions, spot opportunities, and outmaneuver his rivals. His unique mathematical perspective allowed him to discern **patterns** and connections others might have overlooked.

Berezovsky co-authored a book titled "The Secretary Problem: Theory, Applications, and Extensions," with fellow mathematicians Leonid Levin and Yurii Safarov, published in 1986.

The book provides a comprehensive exploration of the **secretary problem**, and optimal stopping, covering its mathematical foundations and real-world applications. Additionally, it delves into various extensions of the problem, such as hiring multiple secretaries or hiring secretaries with different skill sets.

**Optimal stopping** is the art of making informed decisions based on accumulated knowledge when faced with random events.

Let's play a game, and the game is simple. You write down two different numbers on separate pieces of paper and place them face down. Then the other player flips one of the numbers and informs you whether it is greater or smaller than the hidden number.

Astonishingly, it's possible to correctly guess the hidden number more than half the time.

How is this possible?

In this case, the information on whether the flipped number is greater or smaller than the hidden number gives everyone a slight edge.

In fact if we are not allowed to turn over any of the pieces of paper. The odds of guessing which one has the highest number on it are 50/50. So the chances of getting the right answer are the same as flipping a coin.

But when we see one of the numbers, however, the odds will improve if we take the following steps:

1) Generate a random number z.
2) If z is less than the number turned over, declare that the number turned over is the highest.
3) If z is more than the number turned over, declare that the number that remains face down is the highest.

The strategy, in other words, is to go with the number we see, unless the random number k is larger. In this case, it's better to switch to the one we have not seen.

To understand why this strategy gives an advantage, we must consider the value of z about the two numbers written on the papers.
There are three possibilities:

(i) z is smaller than both numbers;
(ii) z is larger than both numbers;
(iii) z falls between the two.

In the first scenario, regardless of the number seen, we stick with it, resulting in a 50/50 chance of being correct.
In the second scenario, regardless of the number seen, we choose the other one, again leading to a 50/50 chance.
The fascinating situation is the third one, where there is a win every time. If we see the lower number, we switch, and if we see the higher number, we stick with it.

Whenever the random number happens to fall between the two numbers written, there will be always a correct guess!

The **game** is known as the secretary problem, and any strategy to solve it was first proposed by the Hungarian mathematician Abraham Wald in the 1940s.

Let's see how it looks in F#:

```fsharp
open System


let random (list: 'a list) =
    let rnd = Random()
    let index = rnd.Next(list.Length)
    list.[index]


let rec remove element list =
  match list with
  | [] -> []
  | h::t when h = element -> t
  | h::t -> h::remove element t


let game() =
    let numbers = [1..100]


    let first = random numbers
    let second =  remove first numbers |> random
    let cards = [first;second]
    let others = remove second (remove first numbers)
    let turnOver = random cards


    let z = random others // 1. Generate a random number


    let guess = match z with
                | s when (s < turnOver) -> turnOver
                | s when (s > turnOver) ->
                    (remove turnOver cards) |> List.head
                | _ -> failwith "it shouldn't happen"


    let max = max first second
    guess = max 


let n = [1..1000]
let wins = n |> List.filter (fun _ -> game() = true)  
printfn "You win %A times out of %A" wins.Length 1000
// You win 667 times out of 1000
// You win 671 times out of 1000
```
