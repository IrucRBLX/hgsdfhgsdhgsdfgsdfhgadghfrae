local tool = script.Parent
local user

--when the tool is equipped
tool.Equipped:connect(function(mouse)
   --store the character of the person using the tool
   user = tool.Parent 

   --when the left mouse button is clicked
   mouse.Button1Down:connect(function() 
       --make and do a hit test along the ray
       local ray = Ray.new(tool.Handle.CFrame.p, (mouse.Hit.p - tool.Handle.CFrame.p).unit*300)
       local hit, position = game.Workspace:FindPartOnRay(ray, user)

       --do damage to any humanoids hit
       local humanoid = hit and hit.Parent and hit.Parent:FindFirstChild("Humanoid")
       if humanoid then
           humanoid:TakeDamage(30)
       end

       --draw the ray
       local distance = (position - tool.Handle.CFrame.p).magnitude
       local rayPart = Instance.new("Part", user)
       rayPart.Name          = "RayPart"
       rayPart.BrickColor    = BrickColor.new("Bright red")
       rayPart.Transparency  = 0.5
       rayPart.Anchored      = true
       rayPart.CanCollide    = false
       rayPart.TopSurface    = Enum.SurfaceType.Smooth
       rayPart.BottomSurface = Enum.SurfaceType.Smooth
       rayPart.formFactor    = Enum.FormFactor.Custom
       rayPart.Size          = Vector3.new(0.2, 0.2, distance)
       rayPart.CFrame        = CFrame.new(position, tool.Handle.CFrame.p) * CFrame.new(0, 0, -distance/2)

       --add it to debris so it disappears after 0.1 seconds
       game.Debris:AddItem(rayPart, 0.1)
   end)
end)
