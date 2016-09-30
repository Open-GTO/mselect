# mselect
 Model Select with previews.

# Screens
Skin selection:
![skin select](https://cloud.githubusercontent.com/assets/1020099/18995206/d367e8d4-8733-11e6-8392-68e3f49110ca.jpg)

# Functions
#### Show created MSelect
```Pawn
MSelect_Show(playerid, function[])
```

#### Open MSelect
```Pawn
stock MSelect_Open(playerid, function[], items_array[], items_count, bool:list_loop = MSELECT_DEF_LIST_LOOP,
      header[] = "", button[] = MSELECT_DEF_BUTTON_TEXT,
      button_next[] = MSELECT_DEF_BUTTON_NEXT, button_prev[] = MSELECT_DEF_BUTTON_PREV,
      Float:pos_x = MSELECT_DEF_POS_X, Float:pos_y = MSELECT_DEF_POS_Y,
      Float:button_width = MSELECT_DEF_BUTTON_WIDTH, Float:button_height = MSELECT_DEF_BUTTON_HEIGHT,
      Float:page_button_width = MSELECT_DEF_PBUTTON_WIDTH, Float:page_button_height = MSELECT_DEF_PBUTTON_HEIGHT,
      Float:item_width = MSELECT_DEF_ITEM_WIDTH, Float:item_height = MSELECT_DEF_ITEM_HEIGHT,
      Float:rot_x = MSELECT_DEF_ROT_X, Float:rot_y = MSELECT_DEF_ROT_Y, Float:rot_z = MSELECT_DEF_ROT_Z,
      Float:zoom = MSELECT_DEF_ZOOM, Float:background_padding = MSELECT_DEF_BG_PADDING,
      Float:item_padding = MSELECT_DEF_ITEM_PADDING, Float:button_padding = MSELECT_DEF_BUTTON_PADDING,
      Float:header_padding = MSELECT_DEF_HEADER_PADDING, Float:page_padding = MSELECT_DEF_PAGE_PADDING,
      select_color = MSELECT_DEF_SELECT_COLOR,
      items_bg_colors[MSELECT_MAX_ITEMS] = {MSELECT_DEF_ITEMS_BG_COLOR, ...},
      dialog_bg_color = MSELECT_DEF_DIALOG_BG_COLOR,
      header_fg_color = MSELECT_DEF_HEADER_FG_COLOR,
      page_fg_color = MSELECT_DEF_PAGE_FG_COLOR,
      button_fg_color = MSELECT_DEF_BUTTON_FG_COLOR,
      button_bg_color = MSELECT_DEF_BUTTON_BG_COLOR)
```

#### Close MSelect
```Pawn
MSelect_Close(playerid);
```

#### Is MSelect opened
```Pawn
MSelect_IsOpen(playerid);
```

# Callbacks
Each MSelect has its own handler function, it looks as follows:
```Pawn
MSelectResponse:example_ms(playerid, MSelectType:response, itemid, itemvalue[])
{
    return 1;
}
```
This function is called when a user interacts with MSelect.

**MSelectType** can have these values:
- MSelect_None
- MSelect_Item
- MSelect_Button
- MSelect_ButtonNext
- MSelect_ButtonPrev
- MSelect_Cancel

# Defines
Directive | Default value | Can be redefined
----------|---------------|------------
MSELECT_MAX_ITEMS | 100 | yes
MSELECT_MAX_ITEMS_PER_LINE | 7 | yes
MSELECT_MAX_ITEMS_LINES | 3 | yes
MSELECT_DEF_LIST_LOOP | false | yes
MSELECT_DEF_BUTTON_TEXT | "Cancel" | yes
MSELECT_DEF_BUTTON_NEXT | ">>" | yes
MSELECT_DEF_BUTTON_PREV | "<<" | yes
MSELECT_DEF_POS_X | 85.0 | yes
MSELECT_DEF_POS_Y | 130.0 | yes
MSELECT_DEF_BUTTON_WIDTH | 60.0 | yes
MSELECT_DEF_BUTTON_HEIGHT | 13.0 | yes
MSELECT_DEF_PBUTTON_WIDTH | 30.0 | yes
MSELECT_DEF_PBUTTON_HEIGHT | 13.0 | yes
MSELECT_DEF_ITEM_WIDTH | 60.0 | yes
MSELECT_DEF_ITEM_HEIGHT | 70.0 | yes
MSELECT_DEF_ROT_X | 0.0 | yes
MSELECT_DEF_ROT_Y | 0.0 | yes
MSELECT_DEF_ROT_Z | 0.0 | yes
MSELECT_DEF_ZOOM | 1.0 | yes
MSELECT_DEF_BG_PADDING | 20.0 | yes
MSELECT_DEF_ITEM_PADDING | 2.0 | yes
MSELECT_DEF_BUTTON_PADDING | 5.0 | yes
MSELECT_DEF_SELECT_COLOR | 0xAAAAAAFF | yes
MSELECT_DEF_ITEMS_BG_COLOR | 0x55555599 | yes
MSELECT_DEF_DIALOG_BG_COLOR | 0x00000099 | yes
MSELECT_DEF_HEADER_FG_COLOR | 0xDDDDDDDD | yes
MSELECT_DEF_PAGE_FG_COLOR | 0xDDDDDDDD | yes
MSELECT_DEF_BUTTON_FG_COLOR | 0x888888FF | yes
MSELECT_DEF_BUTTON_BG_COLOR | 0x000000CC | yes
MSELECT_DEF_HEADER_PADDING | 3.0 | yes
MSELECT_DEF_PAGE_PADDING | 3.0 | yes
MSELECT_MAX_ITEMS_ON_LIST | (MSELECT_MAX_ITEMS_PER_LINE * MSELECT_MAX_ITEMS_LINES) | no
MSELECT_MAX_FUNCTION_NAME | 31 | no
MSELECT_INVALID_MODEL_ID |  -1 | no

# Usage
The system provides the ability to create a function to open MSelect, this is useful when multiple calls one list (mostly used when creating nested menus):
```Pawn
MSelectCreate:test(playerid)
{
	static
		items_array[311] = {-1, ...},
		items_count;

	if (items_array[0] == -1) {
		for(new i = 0; i <= sizeof(items_array); i++) {
			if (i == 74) {
				continue;
			}

			items_array[items_count] = i;
			items_count++;
		}
	}

	MSelect_Open(playerid, MSelect:test, items_array, items_count, .header = "Header");
}

MSelectResponse:test(playerid, MSelectType:response, itemid, modelid)
{
	new string[144];
	format(string, sizeof(string), "ID: %d | Type: %d | Item: %d | Model: %d",
	       playerid, _:response, itemid, modelid);
	SendClientMessage(playerid, -1, string);
	if (response == MSelect_Cancel) {
		MSelect_Close(playerid);
	}
	return 1;
}
```
And you should use MSelect_Show to open created MSelect:
```Pawn
MSelect_Show(playerid, MSelect:example_ms);
```
Of course you can not use the system, you can do everything without MSelectCreate.
