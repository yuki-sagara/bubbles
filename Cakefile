fs     = require 'fs'
{exec} = require 'child_process'
jsdom = require 'jsdom'

path_to_bin = '/Users/franzseo/bin/'
closure_compiler = path_to_bin+'compiler.jar'
html_compressor = path_to_bin+'htmlcompressor.jar'
yui = path_to_bin+'yui.jar'

minHtmlReplace = (infile,outfile) ->
  jsdom.env({
    html: fs.readFileSync(infile),
    scripts: [
      'http://code.jquery.com/jquery-1.8.0.min.js'
    ],
  done: (errors, window) ->
    $ = window.$;
    $('link').each((index, element) ->
      elem = @
      if $(elem).attr('min')
        if $(elem).attr('min') is '__REMOVE__'
          $(elem).remove()
        else
          console.log('link replace ')
          console.log('href="'+$(elem).attr('href')+'"')
          $(elem).attr('href', $(elem).attr('min'))
          console.log('with ')
          console.log('href="'+$(elem).attr('href')+'"')
          $(elem).removeAttr('min')
    )
    $('script').each((index, element) ->
      elem = @
      if $(elem).attr('min')
        if $(elem).attr('min') is '__REMOVE__'
          $(elem).remove()
        else
          console.log('script replace ')
          console.log('src="'+$(elem).attr('src')+'"')
          $(elem).attr('src', $(elem).attr('min'))
          console.log('with ')
          console.log('src="'+$(elem).attr('src')+'"')       
          $(elem).removeAttr('min')
     )
    #remove coffee sources
    $('script').each((index, element) ->
      $(elem).removeAttr('coffee')
     )
     source=window.document.doctype.toString()+window.document.innerHTML
     #console.log(source)
     fs.writeFile(outfile, source, (err) ->
       if err
         console.log(err)
       else
         console.log("index.html was saved")
     )
  })

collectCoffee= (infile) ->
  jsdom.env({
    html: fs.readFileSync(infile),
    scripts: [
      'http://code.jquery.com/jquery-1.8.0.min.js'
    ],
  done: (errors, window) ->
    $ = window.$;
    $('script').each((index, element) ->
      elem = @
      r = []
      if $(elem).attr('coffee')
        r.push($(elem).attr('coffee'))
      )
    return r

  })

srcfiles["src/init.coffee","src/InGameObjects.coffee","src/Player.coffee","src/Explosion.coffee","src/Bullet.coffee","src/Enemy.coffee","src/runtime.coffee","src/helper.coffee","src/start.coffee"]


#coffee --watch --compile --output js/lib/ js/src/
task 'watch', 'watch and compile coffeescript', ->
  exec 'coffee --watch --compile --output js/lib/ js/src/', (err, stdout, stderr) ->
    throw err if err
    console.log stdout + stderr
  
task 'compile', 'compile coffeescript', ->
  exec 'coffee --compile --output js/lib/ js/src/', (err, stdout, stderr) ->
    throw err if err
    console.log stdout + stderr

task 'join', 'join & compile coffeescript', ->
  coff = collectCoffee('index.src.html')
  c_string=coffe.join(' ')
  exec 'coffee --join bubbles.js --compile '+c_string, (err, stdout, stderr) ->
    throw err if err
    console.log stdout + stderr


#coffee --join bubbles.js --compile $srcfiles 

task 'dev', 'set up dev envirement', ->
  
task 'minjs', 'closure compile the javascript', ->
  exec 'java -jar "'+closure_compiler+'" --compilation_level SIMPLE_OPTIMIZATIONS --js js/lib/main.js  --js_output_file js/lib/main.min.js', (err, stdout, stderr) ->
    throw err if err
    console.log stdout + stderr
  
task 'mincss', 'minify the css', ->
  exec "java -jar #{yui} -o css/lib/main.min.css css/src/main.css", (err, stdout, stderr) ->
    throw err if err
    console.log stdout + stderr
  
task 'minhtml', 'minify the html', ->
  minHtmlReplace('index.src.html', 'index.html')
  exec "java -jar #{html_compressor} -o index.html index.src.html", (err, stdout, stderr) ->
    throw err if err
    console.log stdout + stderr

task 'build', 'build the app', ->

task 'publish', 'publish the app', ->
  