Monads, Functors, Applicative and Typeclasses
=============================================
Dec 7th 2019

This was some work I was doing with typeclasses and it turned into a monad learning session.  The custom monad I created is the `Result` monad.

```haskell
class MyMonad m where
  flatMap :: m a -> (a -> m b) -> m b
  mreturn  :: a -> m a

data MyMaybe a = MyJust a | MyNothing

instance MyMonad MyMaybe where
  flatMap MyNothing  f = MyNothing
  flatMap (MyJust x) f = f x
  mreturn               = MyJust

data Error = Error String

data Result a = Ok a | Er Error

instance MyMonad Result where
  flatMap (Ok x) f = f x
  flatMap (Er y) _ = Er y
  mreturn          = Ok

instance Functor Result where
  fmap f (Ok x) = Ok (f x)
  fmap f (Er e) = Er e

instance Applicative Result where
  pure     = Ok
  _ <*> _  = Er $ Error "Something bad happend"

instance Monad Result where
  (Ok x) >>= f = f x
  (Er e) >>= f = Er e
  return       = Ok


alright = Ok "This is ok"

bueno = flatMap alright (\x -> Ok $ x ++ " and is bueno")

data Idd a = Identity a

class Iddd a where
  iddd :: a b -> b

instance Iddd Idd where 
  iddd (Identity x) = x


newtype PersonId = PersonId { personId :: Int }

instance Show PersonId where
  show x = show $ personId x

data Person id_field
  = Person
  { _personId :: id_field
  , name :: String
  }

instance Iddd Person where
  iddd (Person _personId _) = _personId

person = Person { _personId = PersonId 1 , name = "Jone" }


newres = do
  arht <- alright
  bon  <- bueno
  return $
    arht ++ " " ++ bon ++ " combined"


badres = do
  arht <- alright
  bad  <- Er $ Error "BAD!!!"
  bon  <- bueno
  return $
    arht ++ " " ++ bon ++ " combined"

main = do
  putStrLn $ show $ iddd person
  case bueno of
    Ok msg -> putStrLn msg
    _      -> putStrLn "malo"
  case newres of
    Ok m -> putStrLn m
    _    -> putStrLn "No Good"
  case badres of
    Ok m         -> putStrLn m
    Er (Error m) -> putStrLn m
```