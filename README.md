# Wasm training

Try wasm in C.


## Hello wasm

https://webassembly.org/getting-started/developers-guide/

```sh
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk

./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh --build=Release

mkdir hello/
cd hello/
cat << EOF > hello.c
#include<stdio.h>
int main(int argc, char ** argv) {
  printf("Hello, world! \n");
}
EOF
emcc hello.c -o hello.html

emrun --no_browser --port 8080 .
```


## Call a C functoin from js

```sh
touch index.html
touch main.cpp
```

`main.html`
```html
<!doctype html>
<html>
<head></head>
<body>
  <input type="file" class="file">
  <input type="button" class="readfile" value="readfile Click!">

  <input type="button" class="sqrt" value="sqrt Click!">

  <script src="./main.js"></script>
  <script>
    var wasm_int_sqrt = Module.cwrap('int_sqrt', 'number', ['number']);
  </script>
  <script>
    document.querySelector('.sqrt')
    .addEventListener('click', function() {
      console.log(int_sqrt(9));
      console.log(int_sqrt(28));
    });
  </script>
</body>
</html>
```

`main.cpp`
```cpp
#include <math.h>

extern "C" {
  int int_sqrt(int x) {
    return sqrt(x);
  }
}
```

```sh
emcc main.cpp -o main.html -s EXPORTED_FUNCTIONS='["_int_sqrt"]' -s EXTRA_EXPORTED_RUNTIME_METHODS='["ccall", "cwarp"]'
ls  # index.html main.cpp main.html main.js main.wasm

python3 -m http.server  # py3
#python -m SimpleHTTPServer 8888  # py2
```


## Wasm with OpenCV in C

Failed link... give up!!!