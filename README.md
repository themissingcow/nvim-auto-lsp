# nvim-auto-lsp

A small plugin that autoconfigures LSPs that are found on your `$PATH`.

The excellent [Mason](https://github.com/williamboman/mason.nvim),
allows for relatively seamless installation and management of LSPs. If
you wish to install them more globally on your system so they are
available to other apps/tools, then it can be more cumbersome to set up.

This plugin checks for specified (or, alternatively, all known) LSPs on
`$PATH`, and configures them for use if found.

By default, it uses [LSP Zero](https://github.com/VonHeikemen/lsp-zero.nvim)
for configuration.

## Installation (lazy.nvim):

Auto-magic:

```lua
{
    "themissingcow/nvim-auto-lsp",
    dependencies = {
        "neovim/nvim-lspconfig",
        "VonHeikemen/lsp-zero.nvim",
    },
    config = true
}
```

If you wish to customize LSP integration:

```lua
{
    "themissingcow/nvim-auto-lsp",
    config = function()

        local lsp_zero = require("lsp-zero")

        require("nvim-auto-lsp").setup({

            -- Provide a specific list of servers (recommended),
            -- reduces startup time.
            servers = {
              "bashls", "zk", "clangd", "ltex", "lua_ls", "pyright",
              "dockerls", "cssls", "yamlls", "cmake", "html",
              "marksman",
            },

            -- Customize options for specific servers
            server_opts = {
                lua_ls = lsp_zero.nvim_lua_ls(),
                ltex = {
                    language = "en-GB",
                    disabledRules = { ["en-GB"] = { "MORFOLOGIK_RULE_EN_GB" } },
                },
            },
        })
    end,
    dependencies = {
        "VonHeikemen/lsp-zero.nvim",
        {
            "neovim/nvim-lspconfig",
            config = function()
                -- Traditional LSP configuration
                end)
            end,
        },
    },
}
```

## Default options

```lua
require("nvim-auto-lsp").setup( {
	init_function = function (server, opts)
		local lspconfig = require("lspconfig")
		local lsp_zero = require("lsp-zero")
		lspconfig[server].setup(lsp_zero.build_options(server, opts))
	end,
	-- servers = {},
	server_opts = {
		-- server = {}
	}
})
```

## Thanks

Many thanks to [William Boman](https://github.com/williamboman/mason.nvim)
for the work on Mason. and the server mappings.
