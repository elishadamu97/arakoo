---
layout: default
title: Blog

---

# **Replicache 11: Adventures in Text Encoding**

**`Tuesday, June 7, 2022`**

<hr>

## **Summary**

Despite the major version bump, this release has only small changes. There are technically breaking changes, but they are extremely unlikely to affect existing users.

**Bonus:** If longreads about cross-platform text encoding minutiae is your jam, you're in for a treat. [See Sort Order](#) Changes below 🤓.


### **🎁 Features**

* Added support for indexes to <code>experimentalWatch</code>. See [ExperimentalWatchIndexOptions.](#)

### **🧰 Fixes**

* Demoted license-related logs at startup to <code>debug</code> level.
* By calling <code>put()</code> inside a <code>scan()</code> iteration, it was possible to deadlock Replicache 11.0.0. This was fixed.


## ⚠️ **Breaking Changes**

* None.

## ⚡️ **Performance**

* No change.

