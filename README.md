# SnakeJs
<script>
    var COLS=26; ROWS=26;

    var EMPTY=0; SNAKE=1; FRUIT=2;

    //Directions
    var LEFT=0, UP=1, RIGHT=2, DOWN=3;

    var grid = {              //игровое поле
        width: null,
        Height: null,
        _grid: null,

        init: function(d, c, r) {  //d -direction(но вообще думаю это то ЧтО мы в ячейки массива ставим, c-columns, r - rows
            this.width = c;
            this.height = r;
            this._grid = [];       //массив/список элементов с двумя координатами
            for (var x=0; x<c; x++){
                this._grid.push([]);
                for(var y=0; y<r; y++){
                    this._grid[x].push(d);  //Метод push() добавляет один или более элементов в конец массива и возвращает новую длину массива.
                }
            }


        },
        set: function(val, x, y){        //метод устанавливающий в грид с коорд x и y значение val
            this._grid[x][y]=val;

        },
        get: function(x, y){          //метод получающий элемент грида стоящий в координатах x и y?
            return this._grid[x][y];

        },
    }

    var snake = {
        direction: null,
        last: null,
        _queue: null,

        init: function(d, x, y){         //инициализация змейки. (3 параметра направл. и координаты)
            this.direction = d;
            this._queue = [];
            this.insert(x, y); //???

        },
        insert: function(x, y){
            this._queue.unshift({x:x , y:y}); //Метод unshift() добавляет один или более элементов в начало массива и возвращает новую длину массива.
            this.last = this._queue[0]; //last это первый элемент в змейке

        },
        remove: function(){     //метод вовзращает змейку с удаленным последним элементом
            return this._queue.pop();  //Метод pop() УДАЛЕТ последний элемент из массива и возвращает его значение.
        }
    }
    function setFood(){
        var empty = [];   //я так понимаю массив пустых координат, на n-м месте две координаты пустой ячейки
        for(var x=0; x < grid.width; x++){
            for(var y=0; y < grid.height; y++){
                if(grid.get(x,y) === EMPTY){         //если элемент массива грид пустой
                    empty.push({x:x, y:y})   //в empty на место x и y записываем координаты?
                }
            }    
        }                                                              
        var randpos = empty[Math.floor(Math.random()*empty.length)];//генерируем произвольный элемент empty, 1-й там, или второй
        grid.set(FRUIT, randpos.x, randpos.y);  // устанавливает в грид элемент фрут в координаты случ. элемента рандпоз 
    }
//Game objects
    var canvas, ctx, keystate, frames;
    function main(){
        canvas = document.createElement("canvas"); //создаем элемент типа канвас в документе
        canvas.width = COLS*20;
        canvas.height = ROWS*20;
        ctx = canvas.getContext("2d"); //getContext(). Этот метод генерирует контекст рисования, который будет связан с указанным холстом.
        document.body.appendChild(canvas);  //добавляю канвас в боди
        
        frames = 0;           //frames объект виндоус
        keystate = {};
        init();
        loop();
    }
    function init(){
        grid.init(EMPTY, COLS, ROWS);

        var sp = {x:Math.floor(COLS/2), y:ROWS-1}; //{} - что значат?
        snake.init(UP, sp.x, sp.y);     //инициализируем змейку
        grid.set(SNAKE, sp.x, sp.y);        //устанавливаем змейку в координаты
        setFood();
    }
    function loop(){
        update();
        draw();

        window.webkitRequestAnimationFrame(loop, canvas);
    }
    function update(){
        frames++;
    }
    function draw(){
        var tw = canvas.width/grid.width;
        var th = canvas.height/grid.height;

        for(var x=0; x < grid.width; x++){
            for(var y=0; y < grid.height; y++){
                switch(grid.get(x, y)){
                    case EMPTY:
                        ctx.fillStyle = "#fff";
                        break;
                    case SNAKE:
                        ctx.fillStyle = "#f0f";
                        break;
                    case FRUIT:
                        ctx.fillStyle = "#f00";
                        break;
                }
                ctx.fillRect(x*tw, y*th, tw, th);    
            } 
        }  
    }
    
    
</script>
