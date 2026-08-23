# AI Policy

## Rules for OpenCode/Nemotron

1. **Do not invent technical decisions** - Only record confirmed project facts
2. **Mark undecided items as TBD** - Never claim implementation unless it exists
3. **Do not modify Flutter application logic** - Documentation foundation only
4. **Do not add dependencies** - Evaluate and approve through proper channels
5. **Do not redesign architecture** - Maintain cross-platform readiness
6. **Follow PROJECT_RULES.md** - All AI actions must comply
7. **Verify code changes** - Never auto-accept code without review
8. **Security first** - Follow SECURITY.md principles
9. **Testing gates** - Ensure flutter test passes after changes
10. **Stability checks** - Ensure flutter analyze and flutter build apk --debug pass

## Prohibited Actions
- Inventing architecture decisions
- Assuming specific packages (state management, database, networking, playback)
- Copying CloudStream source code or architecture
- Making claims about implemented features that don't exist