# 我的defold笔记导航

```lua
function init(self)
	msg.post(".", "acquire_input_focus")

	self.moving = false
	self.input = vmath.vector3()
	self.dir = vmath.vector3(0,1,0)
	self.speed = 50
end

function final(self)
	msg.post(".", "release_input_focus")
end

function update(self, dt)
	if self.moving then
		local pos = go.get_position()
		pos = pos+self.dir*self.speed*dt
		go.set_position(pos)
	end
	self.input.x = 0
	self.input.y = 0
	self.moving = false
end

function fixed_update(self, dt)
	-- This function is called if 'Fixed Update Frequency' is enabled in the Engine section of game.project
	-- Can be coupled with fixed updates of the physics simulation if 'Use Fixed Timestep' is enabled in
	-- Physics section of game.project
	-- Add update code here
	-- Learn more: https://defold.com/manuals/script/
	-- Remove this function if not needed
end

function on_message(self, message_id, message, sender)
	-- Add message-handling code here
	-- Learn more: https://defold.com/manuals/message-passing/
	-- Remove this function if not needed
end

function on_input(self, action_id, action)
	if action_id == hash("U") then
		self.input.y=1
	elseif action_id == hash("D") then
		self.input.y=-1	
	elseif action_id == hash("L") then
		self.input.x=-1	
	elseif action_id == hash("R") then
		self.input.x=1	
	end

	if vmath.length(self.input)>0 then
		self.moving = true
		self.dir = vmath.normalize(self.input)
	end
end

function on_reload(self)
	-- Add reload-handling code here
	-- Learn more: https://defold.com/manuals/hot-reload/
	-- Remove this function if not needed
end

```

1. `init()`当脚本组件在游戏引擎中生效时，将调用此函数。此函数对于游戏对象状态的初始设置很有用。
2. 此行向当前游戏对象发送了一条名为“acquire_input_focus”的消息（“。”是当前游戏对象的简写）。这是一条系统消息，告知引擎向此游戏对象发送输入操作。这些操作将到达此脚本组件的`on_input()`函数中。
3. `self`是对当前组件实例的引用。您可以在 中存储组件实例本地的状态数据`self`。您可以通过使用点符号对表字段变量进行索引，像使用 Lua 表一样使用它。标志变量`moving`用于跟踪玩家是否正在移动。
4. `input`是一个 vector3，即具有 3 个分量的向量：`x`和`y`，`z`它将指向当前 8 个输入方向中的任意一个。它会随着玩家按下箭头键而改变。此向量的 Z 分量未使用，因此其值保持为 0。
5. `dir`是另一个包含玩家所面向方向的 vector3。方向向量与输入向量是分开的，因为如果没有输入且玩家角色不移动，它仍应面向某个方向。
6. `speed`是以每秒像素数表示的移动速度。
7. 当脚本组件从游戏中删除时，会调用此函数`final()`。这种情况发生在容器游戏对象（“玩家”）被删除或游戏关闭时。
8. 该脚本明确释放输入焦点，告诉引擎它不需要更多输入。当游戏对象被删除时，输入焦点会自动释放，因此此行不是必需的，但是为了清晰起见，在此包含。
9. 该`update()`函数每帧调用一次。游戏以每秒 60 帧的速度运行，因此该函数以 1/60 秒的间隔调用。参数变量`dt`包含当前帧间隔——自上次调用该函数以来经过的秒数。
10. 如果`moving`标志为真，则获取当前游戏对象的位置。该函数`go.get_position()`采用一个可选参数，即要获取其位置的游戏对象的 ID。如果没有给出参数，则返回当前游戏对象的位置。
11. 将当前方向向量（按速度和帧间隔缩放）添加到位置。
12. 将游戏对象的位置设置为新位置。
13. 完成计算后，将输入向量设置为 0 并取消设置`moving`标志。
14. 该`on_input()`函数每帧针对所有处于活动状态的映射输入调用一次。参数`action_id`包含输入绑定文件中设置的操作。参数`action`是一个包含输入详细信息的 Lua 表。
15. 对于每个输入方向，将向量的 X 或 Y 分量设置`input`为`self`。如果用户同时按下up arrow和left arrow键，引擎将调用此函数两次，输入向量最终将被设置为`(-1, 1, 0)`。
16. 如果用户按下任意箭头键，输入向量长度将不为零。如果是这样，请设置标志，`moving`以便玩家在 中移动`update()`。脚本不在`on_input()`函数中移动玩家的原因是，收集每帧的所有输入，然后在 中对其采取行动更为简单`update()`。
17. 方向`dir`向量设置为输入的归一化值。`(1, 1, 0)`例如，如果输入向量为，则向量长度大于 1（2 的平方根）。对向量进行归一化可使其长度恰好为 1。如果不进行归一化，对角线移动将比水平和垂直移动更快。当引擎运行该`update()`函数时，任何用户输入都会对向量产生影响`dir`，从而导致玩家移动。



生产实例factory.create("#roket")