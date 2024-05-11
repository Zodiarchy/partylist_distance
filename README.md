GetDistance Function
This function calculates the distance to a party member based on their target index.
```
local function GetDistance(memIdx)
    local index = AshitaCore:GetMemoryManager():GetParty():GetMemberTargetIndex(memIdx);
    if (index == 0) then
        return;
    end    

    local renderFlags = AshitaCore:GetMemoryManager():GetEntity():GetRenderFlags0(index);
    if (bit.band(renderFlags, 0x200) == 0) then
        return;
    end
    if (bit.band(renderFlags, 0x4000) ~= 0) then
        return;
    end

    return math.sqrt(AshitaCore:GetMemoryManager():GetEntity():GetDistance(index));
end
```

Usage in DrawMember
The GetDistance function is called within GetMemberInformation to store the distance in the memberInfo table. This information is then used in DrawMember to display the distance.
```
local function GetMemberInformation(memIdx)
    ...
    memberInfo.distance = GetDistance(memIdx);
    ...
end

local function DrawMember(memIdx, settings)
    ...
    -- Update the distance text
    if (memInfo.distance ~= nil) and (memIdx ~= 0) then    
        memberText[memIdx].distance:SetPositionY(namePosY + (nameSize.cy + 2));
        memberText[memIdx].distance:SetPositionX(namePosX - (jobIconSize + settings.nameTextOffsetX + -420));
        memberText[memIdx].distance:SetText(string.format('%.1f', memInfo.distance));
        memberText[memIdx].distance:SetVisible(true);
    else
        memberText[memIdx].distance:SetVisible(false);
    end
    ...
end
```

In DrawMember, the distance is formatted to one decimal place and displayed only if it is available (memInfo.distance ~= nil) and the member index is not 0 (i.e., not the player themselves). The visibility and text of the distance are dynamically updated based on the member's presence and distance data.
