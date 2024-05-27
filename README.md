# ImGuiHotKey, works with newer version of imgui   

```
Usage:  
static ImHotkeyState HotKey1{};
ImGui::SelectHotKey(E(u8"热键"), &HotKey1);

struct ImHotkeyState
{
    bool bWaitingForKey = false;
    int Hotkey = 0;
};

namespace ImGui
{
    void SelectHotKey(const char* label, ImHotkeyState* state);
    void StyleColorsDarkRed();
}

void ImGui::SelectHotKey(const char* label, ImHotkeyState* state)
{
	ImGui::Text("%s", label);
	ImGui::SameLine();
	if (state->bWaitingForKey) 
	{
		ImGui::Button("...");

		// Check if any key is pressed
		for (int key = ImGuiKey_NamedKey_BEGIN; key < ImGuiKey_NamedKey_END; key++) 
		{
			if (ImGui::IsKeyPressed((ImGuiKey)key)) 
			{
				if (key == ImGuiKey_Escape) 
				{
					// Cancel the hotkey selection
					state->bWaitingForKey = false;
				}
				else 
				{
					// Set the new hotkey
					state->Hotkey = key;
					state->bWaitingForKey = false;
				}
				break;
			}
		}
	}
	else 
	{
		const char* HotKeyName = ImGui::GetKeyName((ImGuiKey)state->Hotkey);
		if (!HotKeyName || state->Hotkey == 0) 
		{
			HotKeyName = "Set Hotkey";
		}
		if (ImGui::Button(HotKeyName))
		{
			state->bWaitingForKey = true;
		}
	}
}

```
