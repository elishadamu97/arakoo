---
layout: default
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

* None


## ⚠️ **Breaking Changes**

* Enforce 5m timeout for the <code>test license key</code>. It was intended and documented that the test license key was only valid for 5 minutes, but we forgot to enforce that in v10. Now after 5m, Replicache prints an error to the console and stops working.

* <code>scan()</code> now iterates keys in order of the UTF-8 code points of the string. Before, keys were iterated using JavaScript’s native sort, which is by UTF-16 code units. It is 
**extremely** unlikely this affects any current users of Replicache. In order to potentially be affected you must be both using non-ascii characters in your Replicache keys, and relying on the order of keys being stable across client and server. See below for details.

## ⚡️ **Performance**

* No change.

## **Sort Order Changes**

## **Background**

Replicache keys are strings.

Most key/value stores define keys to be simple byte arrays, but JavaScript doesn’t have great support for byte arrays so we decided it would be more idiomatic for keys to be strings.

The <code>scan()</code> method iterates entries in order of their keys.

But what order, specifically? There are many ways strings can be sorted. In databases the way that a collection of strings is sorted is called its <code>collation</code>. Through Replicache 10, our collation was just ”the order that JavaScript sorts strings in”, [which is by UTF-16 code units.](#)

## **Problem**
It’s common for Replicache apps to reuse their mutator functions on the server. On the client, the mutators run against Replicache’s local store. On the server, they run against whatever the authoritative backend database is — PostgreSQL for example.

The collations of these two data stores can be different, resulting in these mutators seeing the same set of keys come out of <code>scan()</code> in different orders. Many Replicache customers use PostgreSQL and it does not have a UTF-16 collation option, meaning for those users the sort order would always be different.

Now, it’s not a correctness issue if a Replicache mutator has a different effect on the server than on the client. [It’s a feature](#), and is what allows permissions and conflict resolution to work. A Replicache client always adopts the authoritative server result of a mutation when it is known.

Also the chance of this behavior being hit by customers is very low, because it only happens when certain multibyte unicode code points are used as keys, and when the application is somehow relying on the sort order of the keys being stable across client/server, both of which are uncommon things to do.

But we still did not like this edge case, and felt it would be very confusing if a developer ever hit it.

### **Solution** 

The solution is to make it easy — ideally the default — for servers to sort the same way the Replicache client does.

UTF-8 is the most common encoding today, especially on the web, and is the default in most newer software. Thus, we decided Replicache should sort keys by UTF-8 code point (aka [ucs_basic](#) in the SQL standard) to match this convention.

However we didn’t want to actually store data as UTF-8 on the client because:

1. This would require a copy and encode/decode from JavaScript’s native UTF-16 on the read and write paths, slowing Replicache down significantly.
2. We would lose the ability to explore Replicache’s data visually in browser dev tools. They only display keys as strings if they actually are native JavaScript strings.

To bridge this gap, Erik Arvidsson wrote a [very clever bit of code](#) that compares JavaScript strings as if they were UTF-8 encoded, without actually allocating, copying, or re-encoding them. We’ve open sourced this and it’s [now available on npm](#) for the ~4 other people on earth who have this problem 🙂.