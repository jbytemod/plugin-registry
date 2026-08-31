# JByteMod Plugin Repository

The official plugin index used by [JByteMod Remastered](https://github.com/jbytemod/JByteMod-Remastered). JByteMod reads [`plugins.json`](plugins.json) to display compatible plugins and install versioned release artifacts.

## Adding a plugin

Open a pull request that adds one entry to `plugins.json`. A listed plugin must:

- Be built for JByteMod Remastered 2.11.0 or newer.
- Have a public source repository and a clear README.
- Publish its JAR as an immutable, versioned GitHub release asset.
- Use the fully qualified concrete `Plugin` class name as its `id`.
- Include the SHA-256 hash of the exact release asset.
- Keep runtime dependencies that are not provided by JByteMod inside the plugin JAR.

Generate the artifact hash with:

```powershell
Get-FileHash -Algorithm SHA256 .\target\plugin-name-1.0.0.jar
```

Every field and validation rule is documented in [`schema.json`](schema.json). Update the version, download URL, file name, and hash together when publishing an update.

## Third-party repositories

JByteMod also accepts additional repository URLs from **Plugins > Manage Plugins > Plugin Repository > Repositories**. A repository can be:

- A GitHub repository whose `main` or `master` branch contains `plugins.json`.
- A direct HTTP or HTTPS URL to a compatible JSON index.

The official `jbytemod/plugin-registry` source is built into JByteMod and cannot be removed. Entries from the first repository that defines a plugin ID take precedence, so the official source cannot be silently replaced by a later source.

## Repository format

```json
{
  "schemaVersion": 1,
  "name": "Example Plugin Repository",
  "plugins": [
    {
      "id": "com.example.jbytemod.ExamplePlugin",
      "name": "Example Plugin",
      "version": "1.0.0",
      "author": "example",
      "description": "An example JByteMod plugin.",
      "minimumJByteModVersion": "2.11.0",
      "website": "https://github.com/example/jbytemod-example",
      "downloadUrl": "https://github.com/example/jbytemod-example/releases/download/1.0.0/plugin-example-1.0.0.jar",
      "sha256": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
      "fileName": "plugin-example-1.0.0.jar"
    }
  ]
}
```

JByteMod verifies the hash and that the download is a JAR containing classes before replacing an installed plugin. Registry indexes are cached for offline browsing; plugin binaries are never installed without a successful verified download.
