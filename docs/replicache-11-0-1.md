---
layout: default
title: Blog

---

# **Replicache 11.0.1**

**`Friday, July 22, 2022`**

<hr>

## **Summary**

**Note:** When these release notes were initially published, <code>experimentalPendingMutation</code> was accidentally omitted.

### 🔌 **Install**

```cs
npm add replicache@11.0.1
```

### **🎁 Features**

* Added <code>experimentalPendingMutation</code> to get the current list of pending mutations. This is intended e.g., for warning user about unsaved changes before shutdown.

### **🧰 Fixes**

* Demoted license-related logs at startup to <code>debug</code> level.
* By calling <code>put()</code> inside a <code>scan()</code> iteration, it was possible to deadlock Replicache 11.0.0. This was fixed.


## ⚠️ **Breaking Changes**

* None.

## ⚡️ **Performance**

* No change.
