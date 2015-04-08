# DataFlow

Generate Graphviz documents from a Haskell representation.

```
cabal configure
cabal build
```

## Usage

```haskell
module Main where

import DataFlow.Core
import DataFlow.DFD

main :: IO ()
main = printDfd $
  Diagram "My Diagram" [
    TrustBoundary "browser" "Browser" [
      Process "webapp" "Webapp"
    ],
    TrustBoundary "aws" "Amazon AWS" [
      Process "server" "Web Server",
      Database "logs" "Logs"
    ],
    External "analytics" "Google Analytics",

    Edge "webapp" "server" "Request /" "",
    Edge "server" "logs" "Log" "User IP",
    Edge "server" "webapp" "Response" "User Profile",

    Edge "webapp" "analytics" "Log" "Page Navigation"
  ]
```

Then generate your output with dot.

```bash
runhaskell example.hs | dot -Tsvg > output.svg
```

That should generate something like the following.

![Example Output](https://raw.githubusercontent.com/owickstrom/dataflow/master/example/output.svg)

