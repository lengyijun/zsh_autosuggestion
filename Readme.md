1. install https://github.com/zsh-users/zsh-autosuggestions
2. append to `~/.zshrc`

```
# cd after mkdir
_zsh_autosuggest_strategy_nice() {
        local prev_cmd="$(_zsh_autosuggest_escape_command "${history[$((HISTCMD-1))]}")"


        if [[ "$prev_cmd" == mkdir* ]]; then
                replaced="${prev_cmd/mkdir/cd}"
                typeset -g suggestion="$replaced"
        else
                # echo "String does not start with mkdir"
        fi
}

# PLACEHOLDER 

export ZSH_AUTOSUGGEST_STRATEGY=(nice $ZSH_AUTOSUGGEST_STRATEGY)
```

3. If you want autosuggestion appear on empty prompt, replace `# PLACEHOLDER` with following code

```
# copy from  https://github.com/OskarSzafer/open-cli-copilot/
previous_suggestion=""

 _update_postdisplay() {
     POSTDISPLAY="${suggestion#$BUFFER}"
     zle redisplay
 }
 zle -N _update_postdisplay_widget _update_postdisplay

 TRAPALRM() {
        _zsh_autosuggest_strategy_nice
        if [[ -n "$suggestion" && "$suggestion" != "$previous_suggestion" ]]; then
                zle _update_postdisplay_widget
                previous_suggestion="$suggestion"
        fi
 }
 # Set the timer interval to 1 second
 TMOUT=1
```

