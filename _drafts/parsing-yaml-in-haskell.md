TODO: add categories for this blog entry:
- haskell-recipes
TODO: remember that categories like "haskell" or "yaml" can be inferred by looking at the text in this entry.

In the dependencies include:

```yaml
library:
  dependencies:
  - aeson
  - yaml
```

The imports you need:

```haskell
import           Data.Aeson.Types
import           GHC.Generics
import           Data.Yaml
```

An example data-type:

```haskell
data CobraConfig = CobraConfig
    { buildCommand :: Text
    , benchCommand :: Text
    } deriving (Eq, Show, Generic)
```

Add an instance with some custom field mapping:

```haskell
instance FromJSON CobraConfig where
    parseJSON = genericParseJSON defaultOptions
        { fieldLabelModifier = fieldsMapping
        , omitNothingFields   = True
        }
        where
          fieldsMapping "buildCommand" = "build-command"
          fieldsMapping "benchCommand" = "bench-command"
          fieldsMapping x = x
```

Then read the yaml file as follows:
```haskell
readConfig :: IO CobraConfig
readConfig = do
    res <- decodeFileEither ".cobra.yaml"
    case res of
        Left err -> error (show err)
        Right cfg -> return cfg
```

