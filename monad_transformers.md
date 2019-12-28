Monad Transformers
==================

Dec 8th 2019

ghci 8.6.5


With Maybe and Either
---------------------
Don't overlook the `runMaybeT`
```haskell
import Control.Monad.IO.Class
import Control.Monad.Trans.Maybe
import Control.Monad.Trans.Except
import Control.Monad.Trans.Class


item1 = return $ Just " hello again "
-- item1 = return Nothing

item2 = return $ Just " hello again and again "
-- item2 = return Nothing

-- eitherIO1 = return $ Right "hello from either "
eitherIO1 = return $ Left  "Error"


eitherIO2 = return $ Right "hello from either 2 "

pop :: IO (Maybe String) -> IO (Maybe String) -> IO (Maybe String)
pop x y = runMaybeT $ do
  x2 <- MaybeT x
  y2 <- MaybeT y
  return $ x2 <> y2


doEither :: IO (Either String String) -> IO (Either String String) -> IO (Either String String)
doEither x y = runExceptT $ do
  x2 <- ExceptT x
  y2 <- ExceptT y
  return $
    x2 <> y2

main = do
  msg <- pop item1 item2
  putStrLn $ show msg
  msg2 <- doEither eitherIO1 eitherIO2
  putStrLn $ show msg2
```

ExceptT with Guards
-------------------

```haskell
type Login  = LoginReq  -> IO (Either Text AuthToken)

data LoginReq = 
  LoginReq
  { loginReqDto     :: LoginDto
  , ipAddress       :: Text
  , findAttempts    :: Text -> IO (Either Text [LoginAttempt])
  , findUserByEmail :: Text -> IO (Either Text (Maybe User))
  , matchPassword   :: Text -> Text -> Bool
  }

login :: Login
login req@LoginReq{..} = runExceptT $ do
  loginAttempts <- ExceptT $ findAttempts ipAddress
  guard $ (Prelude.length loginAttempts) > 10 -- figure out how to throw error here
  userOpt       <- ExceptT $ findUserByEmail $ loginReqDto...loginDtoEmail
  case userOpt of
    Nothing -> throwE "User not found"
    Just u  ->
      if matchPassword (loginReqDto...loginDtoPass) (u...userPass)
        then
          return $ AuthToken "todo"
        else
          -- add inserting login attempt
          throwE "Passwords do not match"
```