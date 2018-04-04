---
layout: post
title:  "Maitaining packages in stackage"
date:   2018-03-24 23:40:00 +0100
categories: 
---

When adding yourself as the maintainer of a haskell package on stackage, the
following requirements must be met:

- Meaningful commit message - please not `Update build-constraints.yml`.
- At least 30 minutes have passed since Hackage upload.
- On your own machine, in a new directory, you have successfully run the
  following set of commands (replace `$package` with the name of the package
  that is submitted, `$version` is the version of the package you want to get
  into Stackage):

    ```sh
     stack unpack $package
     # You might want to run `stack update` if the command above fetches an old
     # version of the package.
     cd $package-$version
     stack init --resolver nightly
     stack build --resolver nightly --haddock --test --bench --no-run-benchmarks
    ```

