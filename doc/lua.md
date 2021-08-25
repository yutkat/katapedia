# Lua

## Tips

### 即時関数にする

```lua
      base_dirs = (function()
        local dirs = {}
        return dirs
      end)()
```

### リストをadd

```lua
dirs[#dirs + 1] = {'~/.ghq', max_depth = 5}
```

### テーブルを表示する

```lua
function tprint (tbl, indent)
  if not indent then indent = 0 end
  for k, v in pairs(tbl) do
    formatting = string.rep("  ", indent) .. k .. ": "
    if type(v) == "table" then
      print(formatting)
      tprint(v, indent+1)
    else
      print(formatting .. v)
    end
  end
end
```
