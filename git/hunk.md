# Today (I think) I figured out what constitutes as a "hunk"

![found on a safe google search for "hunk"](https://pp.vk.me/c314220/v314220459/acf6/CtlCAODBCrE.jpg)

_obviously this_

I asked [@BenGitsCode](https://github.com/BenGitsCode) what determines a "hunk" for [interactive staging](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging) for git, and neither of us knew. He tried it out in vim, but it was looking at the entire change as a hunk and then we had to go back to doing real work and we tabled it.

I think I get it now. It looks like the original hunk will be **any change** on the document. If there's no non-change, you cannot split it (and it'll treat `s` like `?`).

![first attempt](https://cl.ly/3R292N2K2m3y/Screen%20Shot%202017-06-30%20at%204.07.08%20PM.png)
![continued](https://cl.ly/34122b1N1D0K/Screen%20Shot%202017-06-30%20at%204.10.01%20PM.png)

That said, if there _is_ a non-change, if you choose to split it into smaller hunks, it will divide it based on the non-change, but you don't see the divide unless you ask it to. So basically `git add -p` => `y` is just the same as `git add`. You'll _only_ see the smaller hunks if you opt for something other than `y` or `n`.

![second attempt](https://cl.ly/0x2k082N1n3J/Screen%20Shot%202017-06-30%20at%204.11.33%20PM.png)
![2nd continued](https://cl.ly/0m1E29421G3t/Screen%20Shot%202017-06-30%20at%204.14.18%20PM.png)

I have a feeling things like `J` only work after at least one `s`, so I might keep playing around with this, but at least I'm not confused anymore.

**Update:**
Ben has apparently been given split hunks as the first prompt before any staged changes, so I tried it out again and if the non-changes are so long git decides to split it, it will split it.

**Second Update**
Oh my god, it's based on the `@@ <change>,<from-file-line-numbers> <change>,<to-file-line-numbers> @@`
![](https://cl.ly/2S3X3Q0D1K2P/Screen%20Shot%202017-06-30%20at%205.02.20%20PM.png)
![](https://cl.ly/2k0J1V092r18/Screen%20Shot%202017-06-30%20at%205.00.57%20PM.png)

Thank you to [this](https://stackoverflow.com/questions/10950412/what-does-1-1-mean-in-gits-diff-output) Stack Overflow question/answer and [this](http://www.gnu.org/software/diffutils/manual/diffutils.html#Detailed-Unified) documentation.

I guess my main takeaway point is that hunks are determined/split by non-changes, and if that non-change is large enough that git would cut it out in the commit diff anyway.
