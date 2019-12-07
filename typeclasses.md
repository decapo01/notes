Typeclasses
===========

Dec 7th 2019 - Initial

GHCi, version 8.6.5

```haskell
{-# LANGUAGE MultiParamTypeClasses #-}
{-# LANGUAGE FunctionalDependencies #-}

class Id a b | a -> b where  --needed funtional depenecy here
  _id :: a ->  b

class Name a where
  _name :: a -> String

data Person
  = Person
  { pId   :: Int
  , pName :: String
  }

instance Id Person Int where
   _id = pId

instance Name Person where
  _name = pName

person = Person 1 "will"

main =
  putStrLn $ (show $ _id person) ++ " " ++ _name person
```