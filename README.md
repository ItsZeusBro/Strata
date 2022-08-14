# GobbledyGook

Gobbledy gook is the subset of schema that is sufficiently generalizable to the extent that we can apply the same methods to it and always be doing something orderly, predictable, and useful with Gobbledy Gook utilities.

Gobbledy gook is based around a "path" abstraction, in which there is payload on the way to a recursive base case that is collected as a part of the steps in a path

This is the path algorithm used by GobbledyGook library:


      paths(schema, pk, path=[], _paths=[]){

        if(Array.isArray(schema)){
          for(var i=0; i<schema.length; i++){
            var val=schema[i];
            if(Array.isArray(val)){
              //push index to path
              var _path = path.slice()
              _path.push(i)
              this.paths(val, pk, _path, _paths)
            }else if(typeof val === 'object'){
              var _path = path.slice()
              _path.push(i)
              this.paths(val, pk, _path, _paths)
            }else{
              path = path.slice()
              path.push({[i]:val})
            }
          }

        }else if(typeof schema === 'object'){
          for(var i=0; i<Object.keys(schema).length; i++){
            var key = Object.keys(schema)[i];
            var val = schema[key]
            if(pk.includes(key)){
              path.push({[key]:val})
            }else{
              var _path = path.slice()
              _path.push(key)
              this.paths(val, pk, _path, _paths)
            }
          }
        }else{
          //base case
          path.push({"base":schema})
          _paths.push(path)
          return
        }
        return _paths
      }
