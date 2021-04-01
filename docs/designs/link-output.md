# Link And Bookmarks Output

Docfx generate `links` information of each files into one single file `.links.json` for future data utilization of downstream service of docfx.

## Links

`links` includes all the links which has been processed by docfx:

- Relative link in source content will be processed by docfx and convert it to absolute link if it's valid, otherwise keep it untouched.

- Absolute link in source content will NOT be processed by docfx

- Bookmark and query will be part of the output URL

- Regarding the user input metadata:

  - `breadcrumb_path` would affect the final page content, so it will be part of `.links.json`

  - `feedback_product_url` does not affect the final page content, not part of `.links.json`

  - `xref` is input for build, bot not exposed to the final content, not part of `.links.json`

| Links in source content | source content examples | Output in `.links.json` |
| ----------------------- | ----------------------- | ---------------------- |
| Relative Link           | ./a.md?key=value#bookmark (valid)          | /basepath/article/a?key=value#bookmark    |
|                         | a/b.md?key=value#bookmark (invalid)        | a/b.md?key=value#bookmark                 |
| Absolute Link           | /basepath/articles/b?key=value#bookmark    | /basepath/articles/b?key=value#bookmark   |
|                         | www.github.com/x?key=value#bookmark        | www.github.com/x?key=value#bookmark       |
|                         | https://docs.com/y?key=value#bookmark      | https://docs/com/y?key=value#bookmark     |

> NOTE: the absolute links converted from relative links by docfx don't contain `locale` and `versioning` information

## Output

`.links.json` file will be generated by docfx to contains all the links information about all the page files in build.

*output data model*:

  |            |                       |
  |----------- |-----------------------|
  | `links`    | Array of *link model* |

*link model*:

  |                         |                                         |
  |-------------------------|----------------------                   |
  | `source_url`            | /dotnet/articles/a                      |
  | `source_moniker_group`  | 55a4a18f941221e4d8924d8ebc96dd6c                                |
  | `target_url`            | /dotnet/articles/b                      |
  | `source_git_url`        | https://github.com/dotnet/articles/a.md |
  | `source_line`           | 10                                      |


Example:

```json
{
    "links":
    [
        {
            "source_url": "/dotnet/articles/a",
            "source_moniker_group": "55a4a18f941221e4d8924d8ebc96dd6c",
            "target_url": "/dotnet/articles/b",
            "source_git_url": "https://github.com/dotnet/articles/a.md",
            "source_line": "10"
        },
        {
            "source_url": "/dotnet/articles/a",
            "source_moniker_group": "55a4a18f941221e4d8924d8ebc96dd6c",
            "target_url": "https://github.com",
            "source_git_url": "https://github.com/dotnet/articles/a.md",
            "source_line": "20"
        },
        {
            "source_url": "/dotnet/articles/a",
            "source_moniker_group": "55a4a18f941221e4d8924d8ebc96dd6c",
            "target_url": "./c.md",
            "source_git_url": "https://github.com/dotnet/articles/a.md",
            "source_line": "30"
        }
    ]
}
```
> Notice: `source_url` should be the root file for inclusion (TOC inclusion and markdown inclusion), and `source_git_url` should be the included file
