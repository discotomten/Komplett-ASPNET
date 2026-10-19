# Komplett-ASPNET

Ett enkelt .NET-konsolprojekt som demonstrerar automatisk semantisk versionering
med [semantic-release](https://semantic-release.gitbook.io/) i CI/CD.

## Automatisk versionering

Varje push till `main` kör pipelinen i
[`.github/workflows/release.yml`](.github/workflows/release.yml). Där anropas
`semantic-release`, som analyserar commit-meddelanden sedan senaste Git-taggen
och bestämmer nästa semantiska version.

| Commit-meddelande | Bump | Exempel |
| --- | --- | --- |
| `fix: ...` | PATCH | `1.5.3` → `1.5.4` |
| `feat: ...` | MINOR | `1.5.3` → `1.6.0` |
| `fix!: ...` / `feat!: ...` | MAJOR | `1.5.3` → `2.0.0` |
| `BREAKING CHANGE:` i commit-body | MAJOR | `1.5.3` → `2.0.0` |
| `docs:`, `chore:`, `ci:` m.fl. | ingen | ingen ny version |

Regler:

- **En version per release** – alla commits i en merge slås ihop och den högsta
  bumpen vinner. `fix`, `feat`, `fix` → en enda MINOR.
- **Första releasen** – finns ingen tagg ännu blir första bumpvärda ändringen `1.0.0`.
- **Ingen kodändring** – bara `docs:`/`chore:` ger ingen ny tagg och ingen artefakt.

### Så funkar det

1. Senaste taggen (`v1.5.3`) är den enda persistenta state:n – ingen fil muteras.
2. semantic-release analyserar commits sedan dess och skapar nästa version.
3. `@semantic-release/exec` kör `dotnet publish -p:Version=<version>` och zippar
   resultatet till `Komplett-ASPNET-<version>.zip`.
4. `@semantic-release/github` skapar en tagg `v<version>`, en GitHub Release och
   bifogar zippen som en version-namngiven artefakt.
5. Taggen blir basen för nästa körning.

### Konfiguration

| Fil | Roll |
| --- | --- |
| `.releaserc.json` | Plugins och beteende (preset `conventionalcommits` för `feat!`/`fix!`) |
| `package.json` | semantic-release + plugins som devDependencies |
| `package-lock.json` | Låsta versioner för reproducerbara byggen (`npm ci`) |
| `.github/workflows/release.yml` | Pipelinen |

### Rekommendation

Använd **squash-merge** och låt PR-titeln följa konventionen (`feat:`/`fix:`/`feat!:`).
Då blir varje PR exakt en commit-rad och därmed en tydlig version-bump.

## Bygg lokalt

```bash
dotnet run
```

## Kör release-verktyget lokalt (torrkörning)

```bash
npm ci
GITHUB_TOKEN=<token> npx semantic-release --dry-run
```
