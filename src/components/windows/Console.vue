<template>
  <textarea @keydown="keydown" @keyup="keyup" @click="setSelectionRange" :disabled="disabled ? true : null" class="console" spellcheck="false"></textarea>
</template>

<script>
import emitter from 'tiny-emitter/instance'

export default {
  data: () => {
    return {
      username: 'root',
      prefix: `@console:$:`,
      command: '',
      typeName: false,
      disabled: true,
      keylist: []
    }
  },
  mounted: function() {
    this.prefix = this.username + this.prefix
    this.login();
  },
  methods: {
    login: function() {
      const $self = this
      const $console = this.$el
      $console.value += `Try to login as ${this.username}`

      window.setTimeout(function () {
        $console.value += ` .`

        window.setTimeout(function () {
          $console.value += ` .`

          window.setTimeout(function () {
            $console.value += ` .\n`

            window.setTimeout(function () {
              $self.setValue(`you're logged in as ${$self.username}!`)
              $self.newLine()
              $self.disabled = false
            }, 1000)
          }, 1000)
        }, 1000)
      }, 1000)
    },
    setValue: function(text) {
      const $console = this.$el
      $console.value += `${text}\n`
    },
    newLine: function() {
      this.$el.value += `${this.prefix} `
    },
    setSelectionRange() {
      const $console = this.$el
      $console.setSelectionRange($console.value.length, $console.value.length)
    },
    keydown: function(e) {
      const $console = this.$el
      let value = $console.value
      let newline = ''

      if (e.code == 'Backspace') {
        newline = value.split('\n')
        if (newline[newline.length - 1] == this.prefix) {
          this.newLine()
        }
      }

      if (e.code == 'Enter') {
        newline = value.split('\n')
        const lastline = newline[newline.length - 1]
        this.command = lastline.replace(`${this.prefix} `, '')
      }
    },
    keyup: function(e) {
      const self = this
      const $console = this.$el
      let value = $console.value
      if (e.key == 'Enter') {

        if (this.type_name) {
          this.type_name = false
          this.username = this.command
          this.prefix = `${this.username}${this.prefix}:`
          this.setValue(`Your name is now ${this.username}`)
          this.newLine()
          return
        }

        let callme = this.command.match(/callme ([a-zA-Z]+)$/)
        if (callme) {
          this.username = callme[1]
          this.prefix = `${this.username}@10.0.0.1:~$:`
          this.setValue(`Your name is now ${this.username}`)
          this.newLine()
          return
        }

        let command = this.command.match(/([a-z0-9-]+) (.*)/)
        let options = ''
        let opts = []
        if(command) {
          opts = [...command[2].matchAll(/-{1,2}[A-Za-z0-9-]+/g)]
          options = command[2].replace(/-{1,2}[A-Za-z0-9-]+/g,'')
          command = command[1]
        } else {
          command = this.command
        }

        let str = ''
        let stop = false
        const maxBG = 2

        switch (command) {
          case 'mkdir':
            if(options.match(/^\s/)) {
              this.setValue(`Folder cant start with empty space.`)
              stop = true
            }
            if(self.$parent.$parent.apps.some((app) => app.desc == options)) {
              this.setValue(`Folder ${options} already exists.`)
              stop = true
            }
            opts.forEach(o => {
              o = o[0].replace(/-+/,'')
              if(o == 'help' || o == 'h') {
                this.setValue('Just type mkdir [foldername] to create a new folder on Desktop.')
                stop = true
              }
            });
            if(!stop) {
              if(!options.match(/\s/)) {
                this.setValue(`Folder ${options} succesfully created!`)
                emitter.emit('create-folder',options)
              } else {
                this.setValue(`Can't create folder [${options}]. Please try another`)
              }
            }
            break
          case 'set-background':
            opts.forEach(o => {
              o = o[0].replace(/-+/,'')
              if(o == 'help' || o == 'h') {
                this.setValue(`Just type set-background [1 - ${maxBG}] to set new background.`)
                stop = true
              }
            });
            if(!stop) {
              if(!Number.isInteger(options) || options > maxBG) {
                this.setValue(`Your value must be a number between 1 and ${maxBG}`)
              }
              else {
                this.setValue(`Your background is now bg-${options}.jpg`)
                emitter.emit('set-background', options)
              }
            }
            break
          case 'help':
            this.setValue('Just type in a command like help, callme john, hi, clear etc.')
            break
          case 'clear':
            $console.value = ''
            break
          case 'penis':
            str = '========3 LOL :D'
            opts.forEach(o => {
              o = o[0].replace(/-+/,'')
              if(o == 'big' || o == 'b') str = '==========================3 BIG PENIS =D'
              if(o == 'small' || o == 's') str = '==3 mini :('
            });
            this.setValue(str)
            break
          case 'hi':
            this.setValue(`Hi! What's your name? Please type name. For example John`)
            this.type_name = true
            break
          case 'reboot':
            this.setValue(`Console will reboot in 3 sek.`)
            this.disabled = true
            window.setTimeout(self.login(), 3000)
            return
          case 'exit':
            this.$emit('close')
            return
          case 'yoda':
            this.setValue("__.-._")
            this.setValue("'-._\"7'")
            this.setValue("/'.-c")
            this.setValue(" |  /T")
            this.setValue("    _)_/LI")
            break
          case 'general-ackbar':
            this.setValue("                            __...------------._")
            this.setValue("                         ,-'                   `-.")
            this.setValue("                      ,-'                         `.")
            this.setValue("                    ,'                            ,-`.")
            this.setValue("                   ;                              `-' `.")
            this.setValue("                  ;                                 .-. \\")
            this.setValue("                 ;                           .-.    `-'  \\")
            this.setValue("                ;                            `-'          \\")
            this.setValue("               ;                                          `.")
            this.setValue("               ;                                           :")
            this.setValue("              ;                                            |")
            this.setValue("             ;                                             ;")
            this.setValue("            ;                            ___              ;")
            this.setValue("           ;                        ,-;-','.`.__          |")
            this.setValue("       _..;                      ,-' ;`,'.`,'.--`.        |")
            this.setValue("      ///;           ,-'   `. ,-'   ;` ;`,','_.--=:      /")
            this.setValue("     |'':          ,'        :     ;` ;,;,,-'_.-._`.   ,'")
            this.setValue("     '  :         ;_.-.      `.    :' ;;;'.ee.    \\|  /")
            this.setValue("      \\.'    _..-'/8o. `.     :    :! ' ':8888)   || /")
            this.setValue("       ||`-''    \\88o\\ :     :    :! :  :`\"\"'    ;;/")
            this.setValue("       ||         \"88o\\;     `.    \\ `. `.      ;,'")
            this.setValue("       /)   ___    `.\"'/(--.._ `.    `.`.  `-..-' ;--.")
            this.setValue("       \\(.=\"\"\"\"\"==.. `'-'     `.|      `-`-..__.-' `. `.")
            this.setValue("        |          `\"==.__      )                    )  ;")
            this.setValue("        |   ||           `\"=== '                   .'  .'")
            this.setValue("        /\\,,||||  | |           \\                .'   .'")
            this.setValue("        | |||'|' |'|'           \\|             .'   _.' \\")
            this.setValue("        | |\\' |  |           || ||           .'    .'    \\")
            this.setValue("        ' | \\ ' |'  .   ``-- `| ||         .'    .'       \\")
            this.setValue("          '  |  ' |  .    ``-.._ |  ;    .'    .'          `.")
            this.setValue("       _.--,;`.       .  --  ...._,'   .'    .'              `.__")
            this.setValue("     ,'  ,';   `.     .   --..__..--'.'    .'                __/_\\")
            this.setValue("   ,'   ; ;     |    .   --..__.._.'     .'                ,'     `.")
            this.setValue("  /    ; :     ;     .    -.. _.'     _.'                 /         `")
            this.setValue(" /     :  `-._ |    .    _.--'     _.'                   |")
            this.setValue("/       `.    `--....--''       _.'                      |")
            this.setValue("          `._              _..-'                         |")
            this.setValue("             `-..____...-''                              |")
            this.setValue("                                                         |")
            this.setValue("                               mGk                       |")
            break
          default:
            this.setValue(`The command [${this.command}] you've typed in doesn't exist! Please try help`)
            break
        }

        this.newLine()
        return
      }

      if (e.code == 'Backspace') {
        let newline = value.split('\n');
        if (newline[newline.length - 1] == this.prefix) {
          this.newLine()
          return
        }
      }
    }
  },
}
</script>

<style>
.console {
  margin: -1rem;
  width: calc(100% + 2rem);
  height: calc(100% + 2rem);
  border: none;
  background: transparent;
  padding: 0.5rem 1rem;
  color: white;
  font-family: monospace;
  font-size: 0.9rem;
  line-height: 1.2rem;
  resize: none;
  background: var(--console-background);
  outline: none;
  box-shadow: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  caret-color: var(--console-scrollbar);
}

.console::-webkit-scrollbar {
  width: 5px;
}

.console::-webkit-scrollbar-track {
  background: transparent;
}

.console::-webkit-scrollbar-thumb {
  background: var(--console-scrollbar);
}

.console::-webkit-scrollbar-thumb:hover {
  background: var(--console-scrollbar);
}
</style>