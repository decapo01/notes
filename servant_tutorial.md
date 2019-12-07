

Servant Tutorial
================
Dec 06 2019

Installation
------------

`stack new servant-demo`

Add Dependencies
----------------

```yaml
# package.yaml

dependencies:
# other dependencies
- servant
- servant-server
- aeson               
- wai                 
- warp 
```

Getting a List of Users
-----------------------

TLDR here's the final code

`src/Users.hs`
```haskell
{-# LANGUAGE DataKinds #-}
{-# LANGUAGE TypeOperators #-}
{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE DeriveGeneric #-}
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


users :: Handler [User]
users = 
  return 
  [ User "1" "bill"
  , User "2" "pam"
  ]

api = users

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

Try `curl http://localhost:8081/users` or enter that into the url of the browser and you should see
```json
[{"userId":"1","userEmail":"bill"},{"userId":"2","userEmail":"pam"}]
```