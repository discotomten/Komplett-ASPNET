## Versionering

Versioner skapas automatiskt av semantic-release vid push till `main`,
baserat på commit-meddelandena sedan senaste release.

### Commit-format

| Commit | Effekt | Exempel |
|---|---|---|
| `fix:` | PATCH | 1.4.2 → 1.4.3 |
| `feat:` | MINOR | 1.4.2 → 1.5.0 |
| `fix!:` / `feat!:` | MAJOR | 1.4.2 → 2.0.0 |
| `docs:` `chore:` `ci:` `refactor:` `test:` `style:` | Ingen release | – |

### Exempel

```bash
# Buggfix → PATCH
git commit -m "fix: handle empty configuration"

# Ny funktion → MINOR
git commit -m "feat: add -v flag to print version"

# Brytande ändring → MAJOR
git commit -m "feat!: rename command line arguments"

# Med scope (valfritt)
git commit -m "fix(parser): correct date parsing"

# Brytande ändring med förklaring i commit-texten
git commit -m "feat: new config format" -m "BREAKING CHANGE: config.json replaced by config.yml"

# Ingen release
git commit -m "docs: update README"
git commit -m "chore: update dependencies"
git commit -m "ci: adjust release workflow"
```

### Släppa en version

```bash
git push origin main
```

Pipelinen räknar ut versionen, bygger programmet och publicerar
en zip under **Releases**. Versionen syns med `-v` eller under
Egenskaper → Detaljer på .exe-filen.