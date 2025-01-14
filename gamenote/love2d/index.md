我的Love2d笔记

创建一个类的方法：

新建文件：    类名.lua

```lua
类名 = Object:extend() //创建类

function 类名:new(x，y，speed)//重写继承
     self.x = x
     self.y = y
     self.speed = speed
     self.image = love.graphics.newImage("Panda.png")
end
function 类名:update(dt)
    //控制
    if love.keyboard.isDown("left") then
        self.x = self.x-self.speed*dt
    end
    if love.keyboard.isDown("Right") then
        self.x = self.x+self.speed*dt
    end
end


function 类名:draw()
    //渲染
    love.graphics.draw(self.image,self.x,self.y)
end
```

接着在主函数main.lua里

```lua
function love.load()
    Object = require"classic"
        require"类名"
        对象 = 类名(x,y,speed)//这里决定初始位置
end

function love.update(dt)
    对象:update(dt)
end

function love.draw()
    对象:draw()
end
```

获取屏幕尺寸

local window = love.graphics.getWidth()

local window = love.graphics.getHeihgt()

自己在别的类里创建的方法直接在主函数写出来然后:对象:方法(参数)就可以了