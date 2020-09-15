# Separation in modules

A module should make sense given the information contained in it and its
dependencies.

Example of data type and associated functions that won't make sense in a
separate module:

```haskell
module Trace where

-- NOTE: we do not define trace events in a separate module because they do not
-- have a meaning without being associated to a trace. For instance, an invalid
-- action is relative to a previous state in a trace. Similarly, an ensuing
-- state assumes a predecessor state in the trace.

-- | Trace events.
data TraceEvent s
  = Invalid !(SUTAct s)
    -- ^ An invalid action in a trace signifies the fact that he action is
    -- invalid when applied to the state that immediately precedes this action.
  | Valid
    { action       :: !(SUTAct s)
    , ensuingState :: !(SUTSt s)
    }

```

If we would extract `TraceEvent` to a separate module, `Trace` would depend on
it. So `TraceEvent` cannot depend on trace (and this wouldn't make sense). So
we cannot refer to the notion of traces in `TraceEvent`. So this module on its
own would not make sense.
