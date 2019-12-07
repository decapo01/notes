Algebraic Data Types and Record Wild Cards
==========================================

Dec 7th 2019 - Initial

GHCi, version 8.6.5

```haskell
{-# LANGUAGE RecordWildCards #-}

data Person
  = Adult
  { name :: String
  , profession :: String
  }
  | Child
  { name :: String
  , grade :: Int
  }
  

desc :: Person -> String

desc p@Adult{} =
  "My name is " ++ name p ++ " and I am a " ++ profession p

desc p@Child{} =
  "My name is " ++ name p ++ " and I'm in the " ++ (show $ grade p) ++ " (rd/th/st) grade"

adult = Adult "Jim" "Developer"

child = Child "Timmy" 3

main = do
  putStrLn $ desc adult
  putStrLn $ desc child
```