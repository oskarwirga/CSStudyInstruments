## Meta
* Project: yank
* Current Stage: No bugs found
* Brief description: yank text to clipboard from a file descriptor

## Updates


### Week 2

**Current Stage: Bug Hunting**

Tried various weird filetypes to no avail.

Tried reading from an infinite source, hangs while reading which may be a usage bug.

### Week 3

**Current Stage: Bug Hunting**

When reading from /dev/urandom, yank will hang.

### Week 4

**Current Stage: No bugs found**

When reading from /dev/urandom, yank hangs which is not a bug, but expected usage.
