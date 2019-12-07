

Servant Tutorial
================
Dec 06 2019

Stack Version 2.1.3

Installation
------------
```
> stack new servant-demo
```

File Structure (pertinent files only)
--------------------------

```
servant-demo
|- package.yaml
|- app
|  |- Main.hs
|- src
   |- Lib.hs
   |- Users.hs
```

Add Dependencies
----------------

```yaml
# package.yaml

dependencies:
# other dependencies
- servant        == 0.16.2
- servant-server == 0.16.2
- aeson          == 1.4.6.0
- wai            == 3.2.2.1
- warp           == 3.2.28 
```

Getting a List of Users
-----------------------

`src/Users.hs`
```haskell
{-# LANGUAGE DataKinds         #-}
{-# LANGUAGE TypeOperators     #-}
{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE DeriveGeneric     #-}
module Users where

import Servant
import GHC.Generics
import Data.Aeson

type API
  = "users" :> Get '[JSON][User]

data User = User
  { userId    :: String
  , userEmail :: String
  }
  deriving (Generic,Show)

instance ToJSON User where
  toJSON (User userId userEmail) =
    object [ "userId"    .= userId
           , "userEmail" .= userEmail
           ]

users =
  [ User "1" "bill"
  , User "2" "pam"
  ]

usersHandler :: Handler [User]
usersHandler = 
  return users

api = usersHandler

usersProxy :: Proxy API
usersProxy = Proxy

app = serve usersProxy api
```

`src/Lib.hs`
```haskell
module Lib
    ( someFunc
    ) where

import Users
import Network.Wai
import Network.Wai.Handler.Warp (run)

someFunc :: IO ()
someFunc = run 8081 app
```

`app/Main.hs`
```haskell
module Main where

import Lib

main :: IO ()
main = someFunc
```

Running
-------

run `stack run` on the command line in root project directory

```console
> stack run
```

Try curl or enter that into the url of the browser

```curl
> curl http://localhost:8081/users
```
and you should see something like
```json
[{"userId":"1","userEmail":"bill"},{"userId":"2","userEmail":"pam"}]
```