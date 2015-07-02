# What

Being able to use a remote directory mount localy (sshfs for example), and use a local copie off git-annex in one folder.

With git-annex you have a symlink for all file in the git-annex repo, and if the file is not get on your current remote, the symlink is broken.
With a network mount on your git-annex remote, that have all the file you can open and edit all file

## Example

I have a git annex remote with 3 file like this:

- remote-annex/ # Mount with sshfs
	a
	b
	c

- local-annex/
	a -> broken symlink
	b -> path/to/file
	c -> broken symlink

So on this folder i can only use b file, i need to checkout the other file in order to use them

The goal of the project is to do:
- sshannex/
	a -> remote-annex/a
	b -> local-annex/b
	c -> remote-annex/c

Here i can read all file(sshannex use in priority local file, but if not found use the file trough the network)
i can do 
> git annex get sshannex/a

and then my sshannex folder look like 
- sshannex/
	a -> local-annex/a
	b -> local-annex/b
	c -> remote-annex/c

# Why

When at home i like to access all my file, if found on my laptop using the local file, else using the file throw network, transparently
I also like to choose what file i have a copy localy, for when i travel and don't have internet(here it's gitannex who do this for me)

# How

The first POC will be build using rust and rust-fuse(https://github.com/zargony/rust-fuse/)

# Building

Currently you need to have rust nightly, you can install it with:

> curl -sSf https://static.rust-lang.org/rustup.sh | sh -s -- --channel=nightly

When rust and cargo are install, you should be able to  do:

> cargo build

after the build you have a binay with debug symbol, you can use it like:

> mkdir /tmp/test-sshannex
> ./target/debug/sshannex /tmp/test-sshannex &
> ls /tmp/sshannex

# Current State

For now there is only an working example with fuse
