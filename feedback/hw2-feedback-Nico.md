### ***Project 2 Feedback***

***Nico Van de Bovenkamp***

***

**Overall:**  
Great work on this assignment! As you said, I think things are starting to click for you. You might have a bit to learn when it comes to some python fundamentals (like your loops, iterables, data structures, etc.), but you're definitely there with the logic and Pandas!

**Some notes**  

* Two quick points when you do the `.fillna()` function.  
    - First, as you know, when you have nulls you want to either drop them or impute values in some way. You chose to fill them, which is a solid technique. However, you want to fill the missing value with some other value that will not disturb the distribution of the data. One solid way to do so is to to pass the mean or median. Thus. you can do:
    `df['gre'] = df_raw['gre'].fillna(np.mean(df['gre']))` (or something like that!)
    - The second point is on returning objects in functions. Notice in the snippet above that the function `.fillna()` returns a new dataframe that you have altered. This means that you have to store a new variable with that new object! Thus, you catch the object. Be mindful of what methods return objects, and which do not (for example, our `model.fit()` does not return a new object!)
