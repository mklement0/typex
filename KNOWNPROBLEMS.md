# Known Problems

* Inspecting device files such as `/dev/stdin` and `/dev/stdout` doesn't work properly.
   * macOS: `/dev/stdin` -> `/dev/fd/0` is reported as an empty file rather than as a character-special file.
   * Unix: `/dev/stdin` and `/dev/stdout` complain about missing a missing temp file / pipe and do not follow the chain of symlinks to the end.

   