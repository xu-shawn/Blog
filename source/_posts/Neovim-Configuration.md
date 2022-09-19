---
title: Neovim Configuration
date: 2022-09-18 04:07:46
tags:
- Neovim
- DIY
- Linux
categories:
- Linux
---
# Neovim

[Neovim](https://github.com/neovim/neovim) is a Vim-fork focused on extensibility and usability.

It supported using [lua](https://www.lua.org/) to extend its function.

## Install Extension Manager

> Linux & MacOS Installation

```shell
git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

> Windows Installation

```shell
git clone https://github.com/wbthomason/packer.nvim ~\AppData\Local\nvim-data\site\pack\packer\start\packer.nvim
```

## Create files and directories

In order to make the code easy to read and change, we can separate them into different parts.

```shell
.
├── init.vim
├── lua
│   ├── basic.lua
│   ├── keybindings.lua
│   ├── plugin-config
│   │   └── ...
│   └── plugins.lua
└── plugin
    └── packer_compiled.lua
```

- `init.vim` is used to manage all other parts of the configuration
- `basic.lua` is used to config the basic setting ( such as number line )
- `keybinding.lua` is used to config all keybinding
- All files in the`plugin-config` directory is used to config the plugins
- `plugins.lua` is used to manage all the plugins
- `packer_compiled.lua` is the file that contains auto generated packer.nvim loader code

## Basic Configuration

Edit init.vim

```vimscript
" basic settings
lua require('basic')
" keybinding settings
lua require('keybindings')
" plugins
lua require('plugins')
" plugins settings
lua require('plugin-config/nvim-tree')
lua require('plugin-config/bufferline')
lua require('plugin-config/lualine')
" colorscheme
colorscheme dracula
```

Edit basic.lua

```lua
-- set default encoding to utf-8
vim.g.encoding = "UTF-8"
vim.o.fileencoding = 'utf-8'
-- retain 8 lines when moving the cursor
vim.o.scrolloff = 8
vim.o.sidescrolloff = 8
-- use relative number
vim.wo.number = true
vim.wo.relativenumber = true
-- highlight where the cursor is
vim.wo.cursorline = true
-- enable smart case
vim.o.ignorecase = true
vim.o.smartcase = true
vim.o.cmdheight = 2
-- mouse moving support
vim.o.mouse = "a"
-- show space as a point
vim.o.list = true
vim.o.listchars = "space:·"
-- enhance auto complete
vim.o.wildmenu = true
-- always show tabline
vim.o.showtabline = 2
```

## Plugin Installation

Add those plugin config to ./lua/plugins.lua

- Color scheme: [Dracula](https://github.com/Mofiqul/dracula.nvim)
  ```lua
  use 'Mofiqul/dracula.nvim'
  ```

- File manager: [Nvim tree](https://github.com/kyazdani42/nvim-tree.lua)
  ```lua
  use {
  	'kyazdani42/nvim-tree.lua',
  	requires = 'kyazdani42/nvim-web-devicons'
  }
  ```

  Edit `./lua/plugin-config/nvim-tree.lua`

  ```lua
  require'nvim-tree'.setup {
    	-- set auto close
      auto_close = true,
    	-- disable git status
      git = {
          enable = false
      }
  }
  ```

- Tab page integration: [Bufferline](https://github.com/akinsho/bufferline.nvim)

  ```lua
  use {
  	'akinsho/bufferline.nvim',
  	requires = 'kyazdani42/nvim-web-devicons'
  }
  ```

  Edit `./lua/plugin-config/nvim-tree.lua`

  ```lua
  -- open color display
  vim.opt.termguicolors = true
  require("bufferline").setup {
      options = {
          -- use lsp of nvim
          diagnostics = "nvim_lsp",
          -- give a space for nvim-tree
          offsets = {{
              filetype = "NvimTree",
              text = "File Explorer",
              highlight = "Directory",
              text_align = "left"
          }}
      }
  }
  ```

- Status line: [Lualine](https://github.com/nvim-lualine/lualine.nvim)
  ```lua
  use {
   'nvim-lualine/lualine.nvim',
   requires = { 'kyazdani42/nvim-web-devicons', opt = true }
  }
  ```

  Edit `./lua/plugin-config/lualine.lua`

  ```lua
  require('lualine').setup()
  ```
  
- Startup page: [Alpha-nvim](https://github.com/goolord/alpha-nvim)
  ```lua
  use {
  	'goolord/alpha-nvim',
  	config = function ()
  		require'alpha'.setup(require'alpha.themes.startify'.config)
  	end
  }
  ```

- Float terminal: [Vim-floaterm](https://github.com/voldikss/vim-floaterm)
  ```lua
  use 'voldikss/vim-floaterm'
  ```

- Search plugin: [Telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)
  ```lua
  use {
   'nvim-telescope/telescope.nvim',
   tag = '0.1.0',
   requires = { 'nvim-lua/plenary.nvim' }
  }
  ```

- Auto completion: [Coc.nvim](https://github.com/neoclide/coc.nvim)
  ```lua
  use {
  	'neoclide/coc.nvim',
  	branch = 'release'
  }
  ```

## Keybindings config

Edit `./lua/keybindings.lua`

### Basic keybinding

- Set leader key to space
  ```lua
  vim.g.mapleader = " "
  vim.g.maplocalleader = " "
  ```

- Alias for commands
  ```lua
  local map = vim.api.nvim_set_keymap
  local opt = {noremap = true, silent = true }
  ```

- Keys for moving to different windows

  - `Alt + h` = move to left windows
  - `Alt + j` = move to down windows
  - `Alt + k` = move to up
  - `Alt + l` = move to right

  ```lua
  map("n", "<A-h>", "<C-w>h", opt)
  map("n", "<A-j>", "<C-w>j", opt)
  map("n", "<A-k>", "<C-w>k", opt)
  map("n", "<A-l>", "<C-w>l", opt)
  ```

### Opening Nvim Tree

`Alt + m` = open nvim tree

```lua
map('n', '<A-m>', ':NvimTreeToggle<CR>', opt)
```

### Moving to different tabs

- `Ctrl + h` = move to left tab
- `Ctrl + l` = move to right tab

```lua
map("n", "<C-h>", ":BufferLineCyclePrev<CR>", opt)
map("n", "<C-l>", ":BufferLineCycleNext<CR>", opt)
```

### Float terminal

`Alt + f` = open float terminal

```
map("n", "<A-f>", ":FloatermNew --autoclose=1 pwsh<CR>", opt)
```

### Telescope

- `<Leader> + f + f` = search for files
- `<Leader> + f + b` = search for tabs
- `<Leader> + f + h` = search for commands

```lua
map("n", "<leader>ff", ":Telescope find_files<CR>", opt)
map("n", "<leader>fb", ":Telescope buffers<CR>", opt)
map("n", "<leader>fh", ":Telescope help_tags<CR>", opt)
```

### Coc.nvim

- `Tab` = move to next auto completion result
- `Shift + Tab` = move to previous auto completion result

```lua
local function t(str)
    return vim.api.nvim_replace_termcodes(str, true, true, true)
end
function _G.smart_tab()
    return vim.fn.pumvisible() == 1 and t'<C-N>' or t'<Tab>'
end
map('i', '<Tab>', 'v:lua.smart_tab()', {expr = true, noremap = true})
function _G.smart_stab()
    return vim.fn.pumvisible() == 1 and t'<C-P>' or t'<S-Tab>'
end
map('i', '<S-Tab>', 'v:lua.smart_stab()', {expr = true, noremap = true})
```

# Finish

Run `:PackerSync` and then use nvimin your work.

You can also add custom programming language support by installing plugin of Coc.nvim. (Install by `:CocInstall PluginName` command)