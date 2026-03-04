+++
date = '2025-09-09T14:31:23+02:00'
title = 'win32'
menus = 'main'
type = 'page'
+++

# win32 programming

Here's the page that gathers all my `win32` programming. Most likely the title is not accurate right now as I use 64-bit Windows now (since 2010 I guess), but hey, _everyone_ knows what's this all about.

Instead of reinventing the wheel I decided to gather here all the commonly used techniques of Windows programming

## Changing screen resolution

This snippet works:

```c++
DEVMODE dmScreenSettings;
memset (&dmScreenSettings, 0, sizeof (dmScreenSettings));
dmScreenSettings.dmSize = sizeof (dmScreenSettings);
dmScreenSettings.dmPelsWidth = 1280;
dmScreenSettings.dmPelsHeight = 720;
dmScreenSettings.dmBitsPerPel = 32;
dmScreenSettings.dmFields = DM_BITSPERPEL | DM_PELSWIDTH | DM_PELSHEIGHT;
ChangeDisplaySettings(&dmScreenSettings, CDS_FULLSCREEN);
```

## Topmost, fullscreen window

This snippet works:

```c++
  MONITORINFO mi = { sizeof(mi) };
  GetMonitorInfo(MonitorFromWindow(okno, MONITOR_DEFAULTTOPRIMARY), &mi);

  HWND hWnd = CreateWindowEx(
      0,
      MainWndClass,
      MainWndClass,
      WS_POPUP,
      mi.rcMonitor.left,
      mi.rcMonitor.top,
      mi.rcMonitor.right - mi.rcMonitor.left,
      mi.rcMonitor.bottom - mi.rcMonitor.top,
      NULL,
      NULL,
      instancja,
      NULL);
  SetWindowLongPtr(okno, GWL_EXSTYLE,  WS_EX_TOPMOST);
```
