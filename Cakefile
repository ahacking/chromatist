fs = require 'fs'
CoffeeScript = require 'coffee-script'
jade = require 'jade'
chromatist = require './lib/chromatist'

log = console.log

# ANSI Terminal Colors.
ansi =
    boldblue  : '\033[1;34m'
    boldgreen : '\033[1;32m'
    reset     : '\033[0m'

source_dir = 'src/'
index_source = 'chromatist'
sources = [
    'mathutils', 'underscore_mixins', 'matrix3', 'cie',
    'rgb', 'hsl_hsv', 'cielab', 'ciecam', 'gamut'
]
output_filename = "./lib/chromatist.js"

compile = (filename, options) ->
    source_path = source_dir + filename + '.coffee'
    content = fs.readFileSync source_path, 'utf-8'
    output = "/* #{filename}.coffee */\n"
    output += CoffeeScript.compile content, options
    log "#{ansi.boldgreen}compiled#{ansi.reset} #{source_path}"
    return output

header = """
    /**
     * Chromatist JavaScript library v#{chromatist.VERSION}
     * 
     * Copyright 2011, Jacob Rus
     */
         """

task 'build', 'build the main chromatist library', (options) ->
    output = [compile index_source, {bare: true}] # compile the first script "bare"
    output = output.concat (compile source for source in sources)
    output_script = """
        #{header}
        (function() {
        #{output.join '\n'}
        }).call(this);
        """    
    fs.writeFileSync output_filename, output_script, 'utf-8'
    log "saved compiled source in #{output_filename}"
    log "#{ansi.boldblue}DONE#{ansi.reset}"
