# Config - Post Buffer

**Summary**:
HTTP config option for specifying a size limit (in bytes) for files pushed to the remote system.

**Common Pitfals**:

- when set too low, it can throw 400 during git push command with error message:
  `send-pack: unexpected disconnect while reading sideband packet`

**References**:

- [Official Docs](https://git-scm.com/docs/git-config#Documentation/git-config.txt-httppostBuffer)
