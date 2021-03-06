---
id: faqs
title: Frequently Asked Questions
sidebar_label: Frequently Asked Questions
---

## I just installed swizzin and the dashboard shows that it's using all my RAM. WTF?!

The panel reports three statisitics related to memory consumption: `real`, `cache` and `physical`.

`real` memory usage is considered RAM that cannot be used by other processes, as it has been reserved by the current process.

`cache` takes into consideration things like dirty pages and other transient items in your RAM. These pages can be cleared upon request by the kernel if an application requests to use more RAM.

`physical` is `real + cache`

It's likely that your freshly installed machine simply has a high `cache` usage but low `real` usage. If this is the case (but `physical` still reads 99%), you have nothing to worry about -- this is simply Linux performing a it should. If, instead, your `real` usage is consuming the majority of your RAM, you **do** have problems. Consider using a program such as `top` or `htop` to help you narrow down and identify the rogue application.

## I literally just installed my machine and the dashboard says swizzin is using XXXGB. WHY?! That's simply absurd.

swizzin hasn't used the space, don't worry. By deault, when using the ext4 partition format, the disk reserves 5% of the space in the partition for the potential scenario whereby the disk runs out of space. If this happens, and your whole server is formatted under a root partition scheme (i.e. no separate /home directory), your server will still have some space reserved to perform essential tasks such as (but not limited to): system updates, logging and various other things, such as bash auto(tab)-completion (crazy, right?).

Since the reservation is percentage based, the larger your partition, the higher the reserved space.

You can remove the reserved space on the partiont `sda3` with the following command:

```bash
sudo tune2fs -m 0 /dev/sda3
```

You can also enter other non-zero values to customize the reserved space:

```bash
sudo tune2fs -m 1 /dev/sda3
```

Will reserve 1% instead.

It's unlikely that the partiton on *your* server is sda3, so you'll need to use the command `lsblk` to determine which partition exactly to modify.

## The dashboard states I have 0 out of 0 remaining disk space. What's going on?

Did you install the `quota` package? You need to use the command `setquota` to define the limits per user. The default is undefined.

If you just installed every package just because and you don't actually need quotas, feel free to remove the package with `box remove quota`

