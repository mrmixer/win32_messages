# Description
Header file to get a string representation of windows messages you get from `GetMessage`, `PeekMessage`
or in the `WindowProc`.

# Version
`1` - 18/09/2025

`2` - 10/12/2025 : Try to display registered message names for the 0xC000 -> 0xFFFF range.

# Getting started
- Define `WIN32_MESSAGES_IMPLEMENTATION` in ONE translation unit;
- Include this file;
- call `win32_message_get_first_with_value` to get the first message that correspond to the value
  passed;
- optionnally call `win32_message_get_next_with_value` if you want to see if there are other messages
  with the same value;
- optionnally you can call `win32_message_get_from_string` or `win32_message_get_from_string_l` to get
  the value corresponding to a message string;

# Source

Compilation of window messages from several sources:
- winuser.h;
- https://gitlab.winehq.org/wine/wine/-/wikis/Wine-Developer's-Guide/List-of-Windows-Messages
- https://blog.airesoft.co.uk/2009/11/wm_messages/

Some un-official doc about UAH messages: https://github.com/adzm/win32-custom-menubar-aero-theme
What does UAH stands for ?

Some messages values changed with different versions of Windows. I tried to keep the most recent one.

# License

Public Domain

# Example

```c

#define WIN32_MESSAGES_IMPLEMENTATION
#include "win32_messages.h"

#include <stdio.h>

int main( int argc, char** argv ) {
    
    /* Lists all message in the 0 to 0x0400 range.
    Messages with similar values are on the same line.*/
    
    for ( uint32_t i = 0; i < 0x0400; i++ ) {
        
        win32_message_t* message = win32_message_get_first_with_value( i );
        
        if ( message ) {
            
            printf( "%#06x: %s, ", message->value, message->label );
            
            while ( win32_message_get_next_with_value( &message ) ) {
                printf( "%s, ", message->label );
            }
            
            printf( "\n" );
        }
    }
    
    /* What is the value of WM_USER ? */
    win32_message_t* wm_user = win32_message_get_from_string_l( "WM_USER" );
    printf( "WM_USER: %#06x\n", wm_user->value );
    
    return 0;
}

```