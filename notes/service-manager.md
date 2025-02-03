# Service Manager

- `systemd` was unfortunetly used to extend where it's systemd operating system instead GNU/Linux. Some distributions have adopted `s6` as service management tool which allows users to choose components instead bundling everything.
- Void linux have come with `runit` as extremally simple solution.
- `dinit` seems to be modernized dependecy-aware and concurrent service startup, other more modern alternatives land in `shepherd`.
