Activity 3
================
Team 1

Today you will be creating and manipulating vectors, lists, and data
frames to uncover a top secret message.

As you work through this document with your Team members, remember to:

-   Ask questions
-   Google is your friend! If an error is confusing, copy it into Google
    and see what other people are saying. If you don’t know how to do
    something, search for it.
-   Just because there is no error message doesn’t mean everything went
    smoothly. Use the console to check each step and make sure you have
    accomplished what you wanted to accomplish.

Do not edit this first R code chunk. This will allow you to knit your
document and view errors within the knitted report.

### Setup

Each of the following R chunks will cause an error and/or do the desired
task incorrectly. Find the mistake, and correct it to complete the
intended action.

Create three vectors that contain: 1) the lower case letters, 2) the
lower case letters, and 3) some punctuation marks.

``` r
lower_case <- c("a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z")

upper_case <- c("A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z")

punctuation <- c(".", ",", "!", "?", "'", "\"", "(", ")", " ", "-", ";", ":")
```

Comment on what you noticed about the errors and how you used this
information to correct the issues.

**Response**:

1.  error on unexpected string constant in upper\_case:there was a
    missing comma between H and I

2.  Error: unexpected string constant in “punctuation - there are three
    quotation marks”"" and i need to escape one special character
    because R is trying to pair up quotations.

Make one long vector containing all the symbols.

``` r
my_symbols <- c(lower_case, upper_case, punctuation)
```

Comment on what you noticed about the errors and how you used this
information to correct the issues.

**Response**:the code was making use of cbind instead of c which is what
is used in R to produce a vector.Cbind produces a matrix and that’s not
what we want.

Turn the `my_symbols` vector into a data frame, with the variable name
“Symbol”.

``` r
my_symbols <- data.frame(my_symbols)
names(my_symbols) <- "Symbol" 
```

Comment on what you noticed about the errors and how you used this
information to correct the issues.

**Response**: 1. R could not find the data frame function because it
didn’t have a dot between data and frame.

2.  Error stating that R could not find the variable symbol and so i put
    quotation marks around symbol and used the variable assignment
    symbol &lt;-

Find the total number of symbols we have in our data frame.

``` r
len <- nrow(my_symbols)
```

Comment on what you noticed about the errors and how you used this
information to correct the issues.

**Response**:the code runs but it doesn’t give the answer that we
want.we need to use nrow instead of length because its a data frame and
not a vector.

5.  Create a new variable in your dataframe that assigns a number to
    each symbol.

``` r
my_symbols$Num <- 1:len
```

Comment on what you noticed about the errors and how you used this
information to correct the issues.

**Response**:The code included a % sign that was causing an error cause
a $ sign should be used instead.

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these Base R commands, feel free to start working on
your project. The remainder of this activity will help to expand these
Base R commands and I will post a solution on Blackboard by the end of
class.

### Decoding the secret message.

This chunk will load up the encoded secret message data and assign it as
a vector. There are no errors here.

``` r
# Read in full csv file
top_secret <- readr::read_csv("data/secret_code.csv", col_names = FALSE)
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   X1 = col_double()
    ## )

``` r
# Pick off only the column X1
top_secret <- top_secret$X1
```

By altering this top secret set of numbers, you will be able to create a
message. Write your own code to complete the steps below.

1.  Add 14 to every number.
2.  Multiply every number by 18, then subtract 257.
3.  Exponentiate every number. (You may need to Google how to this in
    R!)
4.  Square every number.

``` r
top_secret <- top_secret + 14
top_secret <- top_secret * 18 - 257
top_secret <- exp(top_secret)
top_secret <- top_secret ^ 2
```

**HQ UPDATE:** Headquarters has informed you that at this stage of
decoding, there should be 496 numbers in the secret message that are
below 17.

5.  Turn your vector of numbers into a matrix with 5 columns.
6.  Separately from your top secret numbers, create a vector of all the
    even numbers between 1 and 502. Name it “`evens`”. That is, `evens`
    should be an R object that contain the values 2, 4, 6, 8, …, 502.
7.  Subtract the “evens” vector from the first column of your secret
    message matrix.
8.  Subtract 100 from all numbers 18-24th rows of the 3rd column.
9.  Multiply all numbers in the 4th and 5th column by 2.
10. Turn your matrix back into a vector.

``` r
# HQ check
sum(top_secret < 17)
```

    ## [1] 497

``` r
top_secret <- matrix(top_secret, ncol = 5)
evens <- seq(from = 2, to = 502, by = 2)
top_secret[,1] <- top_secret[,1] - evens
top_secret[18:24,3] <- top_secret[18:24,3] - 100
top_secret[,4:5] <- top_secret[,4:5] * 2
top_secret <- c(top_secret)
```

**HQ UPDATE:** Headquarters has informed you that at this stage of
decoding, all numbers in indices 500 and beyond are below 100.

11. Take the square root of all numbers in indices 38 to 465.
12. Round all numbers to the nearest whole number.
13. Replace all instances of the number 39 with 20.

``` r
#HQ update
sum(top_secret[500:length(top_secret)] < 100)
```

    ## [1] 756

``` r
top_secret[38:465] <- sqrt(top_secret[38:465])
top_secret <- round(top_secret)
top_secret[top_secret == 39] <- 20
```

**HQ UPDATE:** Headquarters has informed you that your final message
should have 421 even numbers.

![](README-img/noun_pause.png) **Planned Pause Point**: If things made
sense, feel free to continue on while you wait. Otherwise, contact your
instructor to have them verify your work.

## The secret message!

Use your final vector of numbers as indices for `my_symbols` to discover
the final message, by running the following code:

``` r
#HQ check
sum((top_secret %% 2) == 0 )
```

    ## [1] 421

``` r
stringr::str_c(my_symbols$Symbol[top_secret], collapse = "")
```

    ## [1] "Oh yeah In France a skinny man died of a big disease with a little name By chance his girlfriend came across a needle and soon she did the same At home there are seventeen-year-old boys and their idea of fun Is being in a gang called The Disciples, high on crack, totin' a machine gun Time Time  Hurricane Annie ripped the ceiling off a church and killed everyone inside You turn on the telly and every other story is tellin' you somebody died Sister killed her baby 'cause she couldn't afford to feed it And we're sending people to the moon September my cousin tried reefer for the very first time Now he's doing horse; it's June Time Time  It's silly, no? When a rocket ship explodes And everybody still wants to fly Some say a man ain't happy Unless a man truly dies Oh, why? Time Time  Baby make a speech, star wars fly Neighbors just shine it on But if a night falls and a bomb falls Will anybody see the dawn Time Time  Is it silly, no? When a rocket blows up And everybody still wants to fly Some say man ain't happy, truly Until a man truly dies Oh, why? Oh, why? Sign o' the Times Time Time  Sign o' the times mess with your mind Hurry before it's too late Let's fall in love, get married, have a baby We'll call him Nate, if it's a boy Time Time"

Google the first line of this message, if you do not recognize it, to
see what it is.

Comment on what you noticed about the activity as a whole.

**Response**:one has to careful to follow the laid down R syntax and
call the correct functions in order to achieve the desired result.

Knit, then stage everything listed in your **Git** pane, commit (with a
meaningful commit message), and push to your GitHub repo. Go to GitHub
and verify that your `activity03-r-intro.Rmd` file appears as you
intended it to.

You can now go back to the `README` file.

## Attribution

This activity is based on a lab from [Dr. Kelly
Bodwin](https://www.kelly-bodwin.com/)’s Stat 331 course