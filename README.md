local player = game:GetService("Players").LocalPlayer
local char = Player.Character

local hobbies = {
  ["videogames"] = {"YOMI Hustle","100% Orange Juice","Roblox","Library of Ruina"},
  ["game dev/design"] = {"UIX","GFX","VFX,"LUA Scripting"},
  ["recreational"] = {"Gym","Drawing","Writing"},
}

local energy = 0, mood = 0, money = char.Balance.Value, late = false, homework = {}
local school = game.Workspace.School
local home = game.Workspace.Home

local function WakeUp()
  repeat
    local rand = math.random(-2,8)
    energy += rand
    mood += rand-6
    task.wait(300)
  until energy >= 80
  local toothbrush = home.Bathroom.Toothbrush
  toothbrush.Parent = char["Right Hand"]
  Instance.new("Toothpaste").Parent = toothbrush
  char["Right Hand"].CFrame = char.Head.CFrame + char.Head.LookVector * 1.25
  task.wait(300)
  toothbrush:FindFirstChild("Toothpaste"):Destroy()
  toothbrush.Parent = home.Bathroom
  char.Pajamas.Parent = home.Closet
  home.Closet.Clothes.Parent = char
end

local function CheckTime()
  task.wait(10)
  if game.Workspace:GetServerTimeNow() + ((char.HumanoidRootPart.Position - school.Position).Magnitude / char.Humanoid.WalkSpeed) > school.Classes:GetChildren()[1].startTime.Value then
    late = true
    mood -= 5
  end
end

local function Attend(class)
  for i,v in pairs(class.Seats:GetChildren()) do
    if v:IsEmpty() then
      char.Humanoid:Sit(v)
      break
    end
    task.wait(1)
  end
  game.Players:GetPlayerFromCharacter(class.Teacher).Chatted:Connect(funtion(lecture)
    char.Humanoid:Listen(lecture)
    table.insert(char.Notes,lecture)
  end)
  if table.find({"Computer Science","Math","Physics"},class.Subject.Value) then
    mood += 15
    energy += 5
  end
  energy -= 10
  for i = 1,math.random(1,3) do
    table.insert(homework,class.Assignments:GetChildren()[#(class.Assignments:GetChildren())])
    mood -= 5
  end
end

char.Added:Connect(function()
  WakeUp()
  CheckTime()
  
  char.Humanoid:MoveTo(school)
  money -= 5
  
  for i,v in pairs(school.Classes:GetChildren()) do
    char.Humanoid:Attend(v)
  end

  char.Humanoid:Eat(Instance.new("Lunch"))
  money -= 20
  mood += 30
  task.wait(600)

  char.Humanoid:MoveTo(home)
  money -= 5
  
  repeat
    char.Humanoid:Do(homework[1])
    table.remove(homework,1)
    energy -= 5
    mood -= 5
  until #homework == 0 or game.Workspace:GetServerTimeNow() > char.BedTime.Value + 30

  repeat
    char.Humanoid:Do(hobbies[math.random(1,#hobbies)])
    local rand = math.random(36,72)
    task.wait(rand)*100)
    energy -= 5
    mood += 20
  until game.Workspace:GetServerTimeNow() < char.BedTime.Value

  char.Humanoid:MoveTo(home.Bed)
  for i = 1,math.random(6,8) do
    energy += 12
    task.wait(3600)
  end
  char:Destroy()
end)
