Monad Transformers
==================

Dec 8th 2019

ghci 8.6

```haskell
import Control.Monad.IO.Class
import Control.Monad.Trans.Maybe
import Control.Monad.Trans.Class


item1 = return $ Just " hello again "
-- item1 = return Nothing

item2 = return $ Just " hello again and again "
-- item2 = return Nothing

pop :: IO (Maybe String) -> IO (Maybe String) -> IO (Maybe String)
pop x y = runMaybeT $ do
  x1 <- MaybeT x
  y1 <- MaybeT y
  return $ x1 <> y1

main = do
  msg <- pop item1 item2
  putStrLn $ show msg
```