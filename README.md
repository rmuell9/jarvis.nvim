# jarvis.nvim
Run LLM models in neovim


## Example Claude Config

```lua
return {
    'rmuell9/jarvis.nvim',

    dependencies = { 'nvim-lua/plenary.nvim'},

    config = function()
        local system_prompt =
        'Replace the selected text/code as if you were actually writing it - so no markdown formatting.'

        local helpful_prompt = 'You are the all-knowing oracle who makes no mistakes and is always consise.'

        local jarvis = require 'jarvis'

        local function anthropic_help()
            jarvis.invoke_llm_and_stream_into_editor({
                url = 'https://api.anthropic.com/v1/messages',
                model = 'claude-sonnet-4-20250514',
                api_key_name = 'ANTHROPIC_API_KEY',
                system_prompt = helpful_prompt,
                replace = false,
            }, jarvis.make_anthropic_spec_curl_args, jarvis.handle_anthropic_spec_data)
        end

        local function anthropic_replace()
            jarvis.invoke_llm_and_stream_into_editor({
                url = 'https://api.anthropic.com/v1/messages',
                model = 'claude-opus-4-20250514',
                api_key_name = 'ANTHROPIC_API_KEY',
                system_prompt = system_prompt,
                replace = true,
            }, jarvis.make_anthropic_spec_curl_args, jarvis.handle_anthropic_spec_data)
        end

        vim.keymap.set({ 'n', 'v' }, '<leader>i', anthropic_help, { desc = 'llm anthropic_help' })
        vim.keymap.set({ 'n', 'v' }, '<leader>I', anthropic_replace, { desc = 'llm anthropic' })

    end
}
```
