
```haskell
{-# LANGUAGE OverloadedStrings #-}

import Web.Scotty
import Network.Wai.Middleware.Static
import Views
import Lucid (renderText)

someFunc = scotty 3000 $ do
  middleware (staticPolicy (addBase "static"))
  get "/" $ do
    html $ renderText page
```

```haskell
empty_ :: Html ()
empty_ = ""

page :: Html ()
page =
  html_ $ do
    body_ $ do
      script_ [src_ "./hello.js"] empty_
```

where `hello.js` is in a directory in the root of the project called `/static`