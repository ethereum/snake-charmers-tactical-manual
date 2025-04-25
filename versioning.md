## Dependency Versioning Philosophy

Our goal is to keep dependency versions as open as possible to avoid limiting users of our libraries.

- **No upper bounds** on versions unless there is a known compatibility issue with a newer major version.
- **Lower bounds** should be set to the earliest version that supports the features we use, to maximize compatibility.
- This approach helps ensure our packages work in a wider range of environments and don't unnecessarily constrain downstream users.

> ⚠️ **Exception:** If a newer major version of a dependency introduces breaking changes or known issues with our code, it's okay to set an upper pin temporarily—just make sure to leave a comment explaining why.

> 📚 **Further Reading:**
> - [Bounded version constraints are bad](https://iscinumpy.dev/post/bound-version-constraints/)
> - [The semantics of version numbers](https://bernat.tech/posts/version-numbers/)
> These articles help explain the rationale behind avoiding upper pins and being cautious with lower bounds.

## Version Pinning Guidelines

Follow these practices when setting or updating version pins:

- **Fast-moving internal libraries** (`eth-utils`, `eth-typing`, `eth-account`, possibly `eth-abi`):
  - Set the lower version pin to the most recent major release during each breaking cycle.
  - This ensures we can adopt new features introduced throughout the year.

- **Slower-moving internal libraries** (e.g., crypto libraries or other stable packages we maintain):
  - Only update the lower pin during a breaking cycle if necessary.
  - Stick to the current version unless there's a clear need to change.

- **Third-party libraries** (`requests`, `websockets`, etc.):
  - Upgrade only when required (e.g., security patches, compatibility issues).
  - In practice, we often end up pinning to the latest major version to stay compatible.

> 💡 **Reminder:** Always evaluate the impact of version changes on downstream packages and tests before updating pins.

