---
title: Rozdział 5 — interfejs API klas hostów USBX
description: Dowiedz się więcej o interfejsie API klas hostów USBX.
author: philmea
ms.author: philmea
ms.date: 5/19/2020
ms.service: rtos
ms.topic: article
ms.openlocfilehash: 62750ab4a5540b243665a7a7d9000a0d60c4435313b6de2e1579ae7f1c20fe55
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2021
ms.locfileid: "116790670"
---
# <a name="chapter-5---usbx-host-classes-api"></a>Rozdział 5 — interfejs API klas hostów USBX

Ten rozdział obejmuje wszystkie widoczne interfejsy API klas hostów USBX. Poniższe interfejsy API dla każdej klasy zostały szczegółowo opisane.

- HID, klasa
- CDC-ACM, klasa
- CDC-ECM, klasa
- Klasa magazynu

## <a name="ux_host_class_hid_client_register"></a>ux_host_class_hid_client_register

Zarejestruj klienta HID w klasie HID.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_client_register(
    UCHAR *hid_client_name,
    UINT (*hid_client_handler)
    (struct UX_HOST_CLASS_HID_CLIENT_COMMAND_STRUCT *));
```

### <a name="description"></a>Opis

Ta funkcja służy do rejestrowania klienta HID w klasie HID. Klasa HID musi znaleźć dopasowanie między urządzeniem HID i klientem HID przed żądaniem danych z tego urządzenia.

> [!NOTE]
> Ciąg języka C hid_client_name musi mieć wartość NULL, a długość tego ciągu (bez samego terminatora NULL) nie może być większa **niż UX_HOST_CLASS_HID_MAX_CLIENT_NAME_LENGTH**.

### <a name="parameters"></a>Parametry

- **hid_client_name** Wskaźnik do nazwy klienta HID.
- **hid_client_handler** Wskaźnik do programu obsługi klienta HID.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Zakończono transfer danych
- **UX_MEMORY_INSUFFICIENT** (0x12) Alokacja pamięci dla klienta nie powiodła się.
- **UX_MEMORY_ARRAY_FULL** (0x1a) Maksymalna liczba już zarejestrowanych klientów.
- **UX_HOST_CLASS_ALREADY_INSTALLED** (0x58) Ta klasa już istnieje

### <a name="example"></a>Przykład

```c
UINT status;

/* The following example illustrates how to register a HID client, in
this case a USB mouse, to the HID class. */

status = ux_host_class_hid_client_register("ux_host_class_hid_client_mouse",
    ux_host_class_hid_mouse_entry);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_report_callback_register"></a>ux_host_class_hid_report_callback_register

Rejestrowanie wywołania zwrotnego z klasy HID.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_report_callback_register(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_REPORT_CALLBACK *call_back);
```

### <a name="description"></a>Opis

Ta funkcja służy do rejestrowania wywołania zwrotnego z klasy HID do klienta HID po otrzymaniu raportu.

### <a name="parameters"></a>Parametry

- **Hid** Wskaźnik do wystąpienia klasy HID
- **call_back** Wskaźnik do struktury call_back danych

### <a name="return-values"></a>Wartości zwracane

- **UX_SUCCESS** (0x00) Zakończono transfer danych
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) Nieprawidłowe wystąpienie HID.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x79) w rejestracji wywołania zwrotnego raportu.

### <a name="example"></a>Przykład

```c
UINT status;

/* This example illustrates how to register a HID client, in this case a USB mouse, to the HID class. In this case, the HID client is asking the HID class to call the client for each usage received in the HID report. */

call_back.ux_host_class_hid_report_callback_id = 0;
call_back.ux_host_class_hid_report_callback_function = ux_host_class_hid_mouse_callback;

call_back.ux_host_class_hid_report_callback_buffer = UX_NULL;
call_back.ux_host_class_hid_report_callback_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

call_back.ux_host_class_hid_report_callback_length = 0;

status = ux_host_class_hid_report_callback_register(hid, &call_back);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_periodic_report_start"></a>ux_host_class_hid_periodic_report_start

Uruchom okresowy punkt końcowy dla wystąpienia klasy HID.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_periodic_report_start(UX_HOST_CLASS_HID *hid);
```

### <a name="description"></a>Opis

Ta funkcja służy do uruchamiania okresowego (przerwania) punktu końcowego dla wystąpienia klasy HID powiązanej z tym klientem HID. Klasa HID nie może uruchomić okresowego punktu końcowego, dopóki klient HID nie zostanie aktywowany i dlatego do klienta HID zostanie pozostawione uruchomienie tego punktu końcowego w celu odbierania raportów.

### <a name="input-parameter"></a>Parametr wejściowy

- **Hid** Wskaźnik do wystąpienia klasy HID.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Okresowe raportowanie zostało pomyślnie uruchomione.
- **ux_host_class_hid_PERIODIC_REPORT_ERROR** (0x7A) w raporcie okresowym.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.

### <a name="example"></a>Przykład

```c
UINT status;

/* The following example illustrates how to start the periodic
endpoint. */

status = ux_host_class_hid_periodic_report_start(hid);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_periodic_report_stop"></a>ux_host_class_hid_periodic_report_stop

Zatrzymaj okresowy punkt końcowy dla wystąpienia klasy HID.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_periodic_report_stop(UX_HOST_CLASS_HID *hid);
```

### <a name="description"></a>Opis

Ta funkcja służy do zatrzymania okresowego (przerwania) punktu końcowego dla wystąpienia klasy HID powiązanej z tym klientem HID. Klasa HID nie może zatrzymać okresowego punktu końcowego, dopóki klient HID nie zostanie zdezaktywowany, wszystkie jego zasoby zostaną wyłączone, a tym samym pozostawione klientowi HID w celu zatrzymania tego punktu końcowego.

### <a name="input-parameter"></a>Parametr wejściowy

- **Hid** Wskaźnik do wystąpienia klasy HID.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Okresowe raportowanie zostało pomyślnie zatrzymane.
- **ux_host_class_hid_PERIODIC_REPORT_ERROR** (0x7A) w raporcie okresowym.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID nie istnieje

### <a name="example"></a>Przykład

```c
UINT status;

/* The following example illustrates how to stop the periodic endpoint. */

status = ux_host_class_hid_periodic_report_stop(hid);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_report_get"></a>ux_host_class_hid_report_get

Pobierz raport z wystąpienia klasy HID.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_report_get(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_CLIENT_REPORT *client_report);
```

### <a name="description"></a>Opis

Ta funkcja służy do odbierania raportu bezpośrednio z urządzenia bez konieczności polegania na okresowym punkcie końcowym. Ten raport pochodzi z punktu końcowego kontroli, ale jego leczenie jest takie samo, jak w przypadku okresowego punktu końcowego.

### <a name="parameters"></a>Parametry

- **Hid** Wskaźnik do wystąpienia klasy HID.
- **client_report** Wskaźnik do raportu klienta HID.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Raport został pomyślnie odebrany.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) Raport klienta był nieprawidłowy lub wystąpił błąd podczas transferu.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.
- **UX_BUFFER_OVERFLOW** (0x5d) Podany bufor nie jest wystarczająco duży, aby pomieścić nieskompresowany raport.

### <a name="example"></a>Przykład

```c
UX_HOST_CLASS_HID_CLIENT_REPORT input_report;

UINT status;

/* The following example illustrates how to get a report. */

input_report.ux_host_class_hid_client_report = hid_report;
input_report.ux_host_class_hid_client_report_buffer = buffer;
input_report.ux_host_class_hid_client_report_length = length;
input_report.ux_host_class_hid_client_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

status = ux_host_class_hid_report_get(hid, &input_report);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_report_set"></a>ux_host_class_hid_report_set

Wysyłanie raportu

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_report_set(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_CLIENT_REPORT *client_report);
```

### <a name="description"></a>Opis

Ta funkcja służy do wysyłania raportu bezpośrednio do urządzenia.

### <a name="parameters"></a>Parametry

- **Hid** Wskaźnik do wystąpienia klasy HID.
- **client_report** Wskaźnik do raportu klienta HID.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Raport został pomyślnie wysłany.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) Raport klienta był nieprawidłowy lub wystąpił błąd podczas transferu.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.
- **UX_HOST_CLASS_HID_REPORT_OVERFLOW** (0x5d) Dostarczony bufor nie jest wystarczająco duży, aby pomieścić nieskompresowany raport.

### <a name="example"></a>Przykład

```c
/* The following example illustrates how to send a report. */

UX_HOST_CLASS_HID_CLIENT_REPORT input_report;

input_report.ux_host_class_hid_client_report = hid_report;
input_report.ux_host_class_hid_client_report_buffer = buffer;
input_report.ux_host_class_hid_client_report_length = length;
input_report.ux_host_class_hid_client_report_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

status = ux_host_class_hid_report_set(hid, &input_report);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_mouse_buttons_get"></a>ux_host_class_hid_mouse_buttons_get

Pobierz przyciski myszy.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_mouse_buttons_get(
    UX_HOST_CLASS_HID_MOUSE *mouse_instance,
    ULONG *mouse_buttons);
```

### <a name="description"></a>Opis

Ta funkcja służy do uzyskania przycisków myszy

### <a name="parameters"></a>Parametry

- **mouse_instance** Wskaźnik do wystąpienia myszy HID.
- **mouse_buttons** Wskaźnik do przycisków powrotnych.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Myszy został pomyślnie pobrany.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.

### <a name="example"></a>Przykład

```c
/* The following example illustrates how to obtain mouse buttons. */

UX_HOST_CLASS_HID_MOUSE *mouse_instance;

ULONG mouse_buttons;

status = ux_host_class_hid_mouse_button_get(mouse_instance, &mouse_buttons);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_mouse_position_get"></a>ux_host_class_hid_mouse_position_get

Pobierz położenie myszy.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_mouse_position_get(
    UX_HOST_CLASS_HID_MOUSE *mouse_instance,
    SLONG *mouse_x_position, 
    SLONG *mouse_y_position);
```

### <a name="description"></a>Opis

Ta funkcja służy do uzyskania położenia myszy w x & współrzędnych y.

### <a name="parameters"></a>Parametry

- **mouse_instance** Wskaźnik do wystąpienia myszy HID.
- **mouse_x_position** Wskaźnik do współrzędnej x.
- **mouse_y_position** Wskaźnik do współrzędnej y.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) współrzędne X &amp; Y zostały pomyślnie pobrane.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.

### <a name="example"></a>Przykład

```c
/* The following example illustrates how to obtain mouse coordinates. */

UX_HOST_CLASS_HID_MOUSE *mouse_instance;

SLONG mouse_x_position;
SLONG mouse_y_position;

status = ux_host_class_hid_mouse_position_get(mouse_instance,
    &mouse_x_position, &mouse_y_position);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_keyboard_key_get"></a>ux_host_class_hid_keyboard_key_get

Pobierz klucz klawiatury i stan.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_keyboard_key_get(
    UX_HOST_CLASS_HID_KEYBOARD *keyboard_instance,
    ULONG *keyboard_key, 
    ULONG *keyboard_state);
```

### <a name="description"></a>Opis

Ta funkcja służy do uzyskania klucza klawiatury i stanu.

### <a name="parameters"></a>Parametry

- **keyboard_instance** Wskaźnik do wystąpienia klawiatury HID.
- **keyboard_key** Wskaźnik do kontenera klawiszy klawiatury.
- **keyboard_state** Wskaźnik do kontenera stanu klawiatury.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) klucz i stan zostały pomyślnie pobrane.
- **UX_ERROR** (0xff) Nic do zgłoszenia.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.

Stan klawiatury może mieć następujące wartości.

- **UX_HID_KEYBOARD_STATE_KEY_UP** 0x10000
- **UX_HID_KEYBOARD_STATE_NUM_LOCK** 0x0001
- **UX_HID_KEYBOARD_STATE_CAPS_LOCK** 0x0002
- **UX_HID_KEYBOARD_STATE_SCROLL_LOCK** 0x0004
- **UX_HID_KEYBOARD_STATE_MASK_LOCK** 0x0007
- **UX_HID_KEYBOARD_STATE_LEFT_SHIFT** 0x0100
- **UX_HID_KEYBOARD_STATE_RIGHT_SHIFT** 0x0200
- **UX_HID_KEYBOARD_STATE_SHIFT** 0x0300
- **UX_HID_KEYBOARD_STATE_LEFT_ALT** 0x0400
- **UX_HID_KEYBOARD_STATE_RIGHT_ALT** 0x0800
- **UX_HID_KEYBOARD_STATE_ALT** 0x0a00
- **UX_HID_KEYBOARD_STATE_LEFT_CTRL** 0x1000
- **UX_HID_KEYBOARD_STATE_RIGHT_CTRL** 0x2000
- **UX_HID_KEYBOARD_STATE_CTRL** 0x3000
- **UX_HID_KEYBOARD_STATE_LEFT_GUI** 0x4000
- **UX_HID_KEYBOARD_STATE_RIGHT_GUI** 0x8000
- **UX_HID_KEYBOARD_STATE_GUI** 0xa000

### <a name="example"></a>Przykład

```c
while (1)
{

    /* Get a key/state from the keyboard. */
    status = ux_host_class_hid_keyboard_key_get(keyboard,
        &keyboard_char, &keyboard_state);

    /* Check if there is something. */
    if (status == UX_SUCCESS)
    {

        #ifdef UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE
            if (keyboard_state & UX_HID_KEYBOARD_STATE_KEY_UP)
            {
                /* The key was released. */
            } else
            {
                /* The key was pressed. */
            }
        #endif

        /* We have a character in the queue. */
        keyboard_queue[keyboard_queue_index] = (UCHAR) keyboard_char;

        /* Can we accept more ? */
        if(keyboard_queue_index < 1024)
            keyboard_queue_index++;
    }

    tx_thread_sleep(10);

}
```

## <a name="ux_host_class_hid_keyboard_ioctl"></a>ux_host_class_hid_keyboard_ioctl

Wykonaj funkcję IOCTL na klawiaturze HID.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_keyboard_ioctl(
    UX_HOST_CLASS_HID_KEYBOARD *keyboard_instance,
    ULONG ioctl_function, VOID *parameter);
```

### <a name="description"></a>Opis

Ta funkcja wykonuje określoną funkcję ioctl na klawiaturze HID. Wywołanie jest blokujące i zwracane tylko wtedy, gdy wystąpi błąd lub gdy polecenie zostanie zakończone.

### <a name="parameters"></a>Parametry

- **keyboard_instance** Wskaźnik do wystąpienia klawiatury HID.
- **ioctl_function** funkcję ioctl do wykonania. W poniższej tabeli przedstawiono jedną z dozwolonych funkcji ioctl.
- **parametr** Wskaźnik do parametru specyficznego dla ioctl.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Funkcja ioctl została ukończona pomyślnie.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Unknown IOCTL, funkcja

### <a name="ioctl-functions"></a>Funkcje IOCTL

- UX_HID_KEYBOARD_IOCTL_SET_LAYOUT
- UX_HID_KEYBOARD_IOCTL_KEY_DECODING_ENABLE
- UX_HID_KEYBOARD_IOCTL_KEY_DECODING_DISABLE

### <a name="example--change-keyboard-layout"></a>Przykład — zmienianie układu klawiatury

```c
UINT status;

/* This example shows usage of the SET_LAYOUT IOCTL function.
    USBX receives raw key values from the device (these raw values
    are defined in the HID usage table specification) and optionally
    decodes them for application usage. The decoding is performed
    based on a set of arrays that act as maps – which array is used
    depends on the raw key value (i.e. keypad and non-keypad) and
    the current state of the keyboard (i.e. shift, caps lock, etc.). */

/* When the shift condition is not present and the raw key value
    is not within the keypad value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_unshifted_map[] =
{
    0,0,0,0,
    'a','b','c','d','e','f','g',
    'h','i','j','k','l','m','n',
    'o','p','q','r','s','t',
    'u','v','w','x','y','z',
    '1','2','3','4','5','6','7','8','9','0',
    0x0d,0x1b,0x08,0x07,0x20,'-','=','[',']',
    '\\','#',';',0x27,'`',',','.','/',0xf0,
    0xbb,0xbc,0xbd,0xbe,0xbf,0xc0,0xc1,0xc2,0xc3,0xc4,0xc5,0xc6,
    0x00,0xf1,0x00,0xd2,0xc7,0xc9,0xd3,0xcf,0xd1,0xcd,0xcd,0xd0,0xc8,0xf2,
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
    0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
};

/* When the shift condition is present and the raw key value
    is not within the keypad value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_shifted_map[] =
{
    0,0,0,0,
    'A','B','C','D','E','F','G',
    'H','I','J','K','L','M','N',
    'O','P','Q','R','S','T',
    'U','V','W','X','Y','Z',
    '!','@','#','$','%','^','&','*','(',')',
    0x0d,0x1b,0x08,0x07,0x20,'_','+','{','}',
    '|','~',':','"','~','<','>','?',0xf0,
    0xbb,0xbc,0xbd,0xbe,0xbf,0xc0,0xc1,0xc2,0xc3,0xc4,0xc5,0xc6,
    0x00,0xf1,0x00,0xd2,0xc7,0xc9,0xd3,0xcf,0xd1,0xcd,0xcd,0xd0,0xc8,0xf2,
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
    0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
};

/* When numlock is on and the raw key value is within the keypad
    value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_numlock_on_map[] =
{
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
};

/* When numlock is off and the raw key value is within the keypad value
    range, this array will be used to decode the raw key value. */
static UCHAR keyboard_layout_raw_to_numlock_off_map[] =
{
    '/','*','-','+',
    0x0d,0xcf,0xd0,0xd1,0xcb,'5',0xcd,0xc7,0xc8,0xc9,0xd2,0xd3,'\\',0x00,0x
    00,'=',
};

/* Specify the keyboard layout for USBX usage. */
static UX_HOST_CLASS_HID_KEYBOARD_LAYOUT keyboard_layout =
{
    keyboard_layout_raw_to_shifted_map,
    keyboard_layout_raw_to_unshifted_map,
    keyboard_layout_raw_to_numlock_on_map,
    keyboard_layout_raw_to_numlock_off_map,
    /* The maximum raw key value. Values larger than this are discarded. */
    UX_HID_KEYBOARD_KEYS_UPPER_RANGE,
    /* The raw key value for the letter 'a'. */
    UX_HID_KEYBOARD_KEY_LETTER_A,
    /* The raw key value for the letter 'z'. */
    UX_HID_KEYBOARD_KEY_LETTER_Z,
    /* The lower range raw key value for keypad keys - inclusive. */
    UX_HID_KEYBOARD_KEYS_KEYPAD_LOWER_RANGE,
    /* The upper range raw key value for keypad keys. */
    UX_HID_KEYBOARD_KEYS_KEYPAD_UPPER_RANGE
};

/* Call the IOCTL function to change the keyboard layout. */
status = ux_host_class_hid_keyboard_ioctl(keyboard,
    UX_HID_KEYBOARD_IOCTL_SET_LAYOUT, (VOID *)&keyboard_layout);

/* If status equals UX_SUCCESS, the operation was successful. */
```

### <a name="example--disable-keyboard-key-decode"></a>Przykład — wyłączanie dekodowania klawisza klawiatury

```c
UINT status;

/* The following example illustrates IOCTL function of Disable key decode from keyboard layout. */
status = ux_host_class_hid_keyboard_ioctl(keyboard,
    UX_HID_KEYBOARD_IOCTL_DISABLE_KEYS_DECODE, UX_NULL);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_remote_control_usage_get"></a>ux_host_class_hid_remote_control_usage_get

Uzyskiwanie użycia zdalnego sterowania

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_hid_remote_control_usage_get(
    UX_HOST_CLASS_HID_REMOTE_CONTROL *remote_control_instance,
    LONG *usage, 
    ULONG *value);
```

### <a name="description"></a>Opis

Ta funkcja służy do uzyskania użycia zdalnego sterowania.

### <a name="parameters"></a>Parametry

- **remote_control_instance** Wskaźnik do wystąpienia zdalnego sterowania HID.
- **użycie** Wskaźnik do użycia.
- **wartość** Wskaźnik do wartości użycia.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Zakończono transfer danych.
- **UX_ERROR** (0xff) Nic do zgłoszenia.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) klasy HID nie istnieje.

Lista wszystkich możliwych zastosowań jest zbyt długa, aby zmieściła się w tym podręczniku użytkownika. Aby uzyskać pełny opis, ux_host_class_hid.h zawiera cały zestaw możliwych wartości.

### <a name="example"></a>Przykład

```c
/* Read usages and values as the user changes the vol/bass/treble buttons on the speaker */

while (remote_control != UX_NULL)
{
    status = ux_host_class_hid_remote_control_usage_get(remote_control, &usage, &value);
    if (status == UX_SUCCESS)
    {
        /* We have something coming from the HID remote control,
        we filter the usage here and only allow the
        volume usage which can be VOLUME, VOLUME_INCREMENT or VOLUME_DECREMENT */
        switch(usage)
        {
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME :
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME_INCREMENT :
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME_DECREMENT :

            if (value<0x80)
            {
                if (current_volume + audio_control.ux_host_class_audio_control_res < 0xffff)
                    current_volume = current_volume + audio_control.ux_host_class_audio_control_res;
                } else {
                if (current_volume > audio_control.ux_host_class_audio_control_res)
                    current_volume = current_volumeaudio_control.ux_host_class_audio_control_res;
            }

            audio_control.ux_host_class_audio_control_channel = 1;
            audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
            audio_control.ux_host_class_audio_control_cur = current_volume;
            status = ux_host_class_audio_control_value_set(audio, &audio_control);
            audio_control.ux_host_class_audio_control_channel = 2;
            audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
            audio_control.ux_host_class_audio_control_cur = current_volume;
            status = ux_host_class_audio_control_value_set(audio, &audio_control);
            break;

        }
    }
    tx_thread_sleep(10);

}
```

### <a name="ux_host_class_cdc_acm_read"></a>ux_host_class_cdc_acm_read

Odczyt z interfejsu cdc_acm interfejsu.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_cdc_acm_read(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length);
```

### <a name="description"></a>Opis

Ta funkcja odczytuje z interfejsu cdc_acm interfejsu. Wywołanie jest blokujące i zwracane tylko wtedy, gdy wystąpi błąd lub gdy transfer zostanie ukończony.

> [!Note]
> Ta funkcja odczytuje nieprzetworzone dane zbiorcze z urządzenia, więc pozostaje w stanie oczekiwania do momentu zapełnienia buforu lub zakończenia transferu przez krótki pakiet (w tym pakiet o zerowej długości). Aby uzyskać więcej informacji, zapoznaj się z [**tematem Ogólne zagadnienia dotyczące transferu zbiorczego.**](usbx-device-stack-5.md#general-considerations-for-bulk-transfer)

### <a name="parameters"></a>Parametry

- **cdc_acm** Wskaźnik do cdc_acm klasy.
- **data_pointer** Wskaźnik do adresu buforu ładunku danych.
- **requested_length** Długość do odebrania.
- **actual_length** Rzeczywiście odebrana długość.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Zakończono transfer danych.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) Wystąpienie cdc_acm jest nieprawidłowe.
- **UX_TRANSFER_TIMEOUT** (0x5c) Limit czasu transferu, odczyt jest niekompletny.

### <a name="example"></a>Przykład

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_read(cdc_acm, data_pointer,
    requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_cdc_acm_write"></a>ux_host_class_cdc_acm_write

Zapis w interfejsie cdc_acm interfejsu

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_cdc_acm_write(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UCHAR *data_pointer, 
    ULONG requested_length, 
    ULONG *actual_length);
```

### <a name="description"></a>Opis

Ta funkcja zapisuje w interfejsie cdc_acm interfejsie. Wywołanie jest blokujące i zwracane tylko wtedy, gdy wystąpi błąd lub gdy transfer zostanie ukończony.

### <a name="parameters"></a>Parametry

- **cdc_acm** Wskaźnik do cdc_acm klasy.
- **data_pointer** Wskaźnik do adresu buforu ładunku danych.
- **requested_length** Długość do wysłania.
- **actual_length** Długość rzeczywiście wysłana.

### <a name="return-values"></a>Wartości zwrócone

- **UX_SUCCESS** (0x00) Zakończono transfer danych.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) Wystąpienie cdc_acm jest nieprawidłowe.
- **UX_TRANSFER_TIMEOUT** (0x5c) Limit czasu transferu, zapis jest niekompletny.

### <a name="example"></a>Przykład

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_write(cdc_acm, data_pointer,
    requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_cdc_acm_ioctl"></a>ux_host_class_cdc_acm_ioctl

Wykonaj funkcję IOCTL w interfejsie cdc_acm interfejsu.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_cdc_acm_ioctl(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function, 
    VOID *parameter);
```

### <a name="description"></a>Opis

Ta funkcja wykonuje określoną funkcję ioctl dla interfejsu cdc_acm interfejsu. Wywołanie jest blokujące i zwracane tylko wtedy, gdy wystąpi błąd lub gdy polecenie zostanie zakończone.

### <a name="parameters"></a>Parametry

- **cdc_acm** Wskaźnik do cdc_acm klasy.
- **ioctl_function** funkcję ioctl do wykonania. W poniższej tabeli przedstawiono jedną z dozwolonych funkcji ioctl.
- **parametr** Wskaźnik do parametru specyficznego dla ioctl

### <a name="return-value"></a>Wartość zwracana

- **UX_SUCCESS** (0x00) Zakończono transfer danych.
- **UX_MEMORY_INSUFFICIENT** (0x12) Za mało pamięci.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) CDC-ACM jest w nieprawidłowym stanie.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Unknown IOCTL, funkcja.

### <a name="ioctl-functions"></a>Funkcje IOCTL:

- UX_HOST_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING
- UX_HOST_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING
- UX_HOST_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE
- UX_HOST_CLASS_CDC_ACM_IOCTL_SEND_BREAK
- UX_HOST_CLASS_CDC_ACM_IOCTL_ABORT_IN_PIPE
- UX_HOST_CLASS_CDC_ACM_IOCTL_ABORT_OUT_PIPE
- UX_HOST_CLASS_CDC_ACM_IOCTL_NOTIFICATION_CALLBACK
- UX_HOST_CLASS_CDC_ACM_IOCTL_GET_DEVICE_STATUS

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_ioctl(cdc_acm,
    UX_HOST_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING, (VOID *)&line_coding);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_cdc_acm_reception_start"></a>ux_host_class_cdc_acm_reception_start

Rozpoczyna odbierania danych z urządzenia w tle.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_cdc_acm_reception_start(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UX_HOST_CLASS_CDC_ACM_RECEPTION *cdc_acm_reception);
```

### <a name="description"></a>Opis

Ta funkcja powoduje, że usbx stale odczytuje dane z urządzenia w tle. Po zakończeniu każdej transakcji wywoływane  jest wywołanie zwrotne określone w cdc_acm_reception, aby aplikacja może wykonywać dalsze przetwarzanie danych transakcji.

> [!NOTE]
> **ux_host_class_cdc_acm_read** nie można używać w trakcie korzystania z odbioru w tle.

### <a name="parameters"></a>Parametry

- **cdc_acm** Wskaźnik do cdc_acm klasy.
- **cdc_acm_reception** Wskaźnik do parametru, który zawiera wartości definiujące zachowanie odbioru w tle. Układ tego parametru jest następujący:

```c
typedef struct UX_HOST_CLASS_CDC_ACM_RECEPTION_STRUCT
{
    ULONG ux_host_class_cdc_acm_reception_state;
    ULONG ux_host_class_cdc_acm_reception_block_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_buffer;
    ULONG ux_host_class_cdc_acm_reception_data_buffer_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_head;
    UCHAR *ux_host_class_cdc_acm_reception_data_tail;
    VOID (*ux_host_class_cdc_acm_reception_callback)(struct UX_HOST_CLASS_CDC_ACM_STRUCT *cdc_acm,
        UINT status, UCHAR *reception_buffer, ULONG reception_size);
} UX_HOST_CLASS_CDC_ACM_RECEPTION;
```

### <a name="return-value"></a>Wartość zwracana

- **UX_SUCCESS** (0x00) Odbiór w tle został pomyślnie uruchomiony.
- **UX_HOST_CLASS_INSTANCE _UNKNOWN** (0x5b) Nieprawidłowe wystąpienie klasy.

```c
UINT status;
UX_HOST_CLASS_CDC_ACM_RECEPTION cdc_acm_reception;

/* Setup the background reception parameter. */

/* Set the desired max read size for each transaction.
    For example, if this value is 64, then the maximum amount of
    data received from the device in a single transaction is 64.
    If the amount of data received from the device is less than this value,
    the callback will still be invoked with the actual amount of data received. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_block_size = block_size;

/* Set the buffer where the data from the device is read to. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_data_buffer = cdc_acm_reception_buffer;

/* Set the size of the data reception buffer.
    Note that this should be at least as large as ux_host_class_cdc_acm_reception_block_size. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_data_buffer_size = cdc_acm_reception_buffer_size;

/* Set the callback that is to be invoked upon each reception transfer completion. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_callback = reception_callback;

/* Start background reception using the values we defined in the reception parameter. */
status = ux_host_class_cdc_acm_reception_start(cdc_acm_host_data, &cdc_acm_reception);

/* If status equals UX_SUCCESS, background reception has successfully started. */
```

## <a name="ux_host_class_cdc_acm_reception_stop"></a>ux_host_class_cdc_acm_reception_stop

Zatrzymuje odbiór pakietów w tle.

### <a name="prototype"></a>Prototype

```c
UINT ux_host_class_cdc_acm_reception_stop(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UX_HOST_CLASS_CDC_ACM_RECEPTION *cdc_acm_reception);
```

### <a name="description"></a>Opis

Ta funkcja powoduje, że usbx zatrzymać odbiór w tle wcześniej uruchomiony przez **ux_host_class_cdc_acm_reception_start**.

### <a name="parameters"></a>Parametry

- **cdc_acm** Wskaźnik do cdc_acm klasy.
- **cdc_acm_reception** Wskaźnik do tego samego parametru, który został użyty do rozpoczęcia odbioru w tle. Układ tego parametru jest następujący:

```c
typedef struct UX_HOST_CLASS_CDC_ACM_RECEPTION_STRUCT
{
    ULONG ux_host_class_cdc_acm_reception_state;
    ULONG ux_host_class_cdc_acm_reception_block_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_buffer;
    ULONG ux_host_class_cdc_acm_reception_data_buffer_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_head;
    UCHAR *ux_host_class_cdc_acm_reception_data_tail;
    VOID (*ux_host_class_cdc_acm_reception_callback)(
            struct UX_HOST_CLASS_CDC_ACM_STRUCT *cdc_acm, UINT status,
            UCHAR *reception_buffer, ULONG reception_size);
} UX_HOST_CLASS_CDC_ACM_RECEPTION;
```

### <a name="return-value"></a>Wartość zwracana

- **UX_SUCCESS** (0x00) Odbiór w tle został pomyślnie zatrzymany.
- **UX_HOST_CLASS_INSTANCE _UNKNOWN** (0x5b) Nieprawidłowe wystąpienie klasy.

```c
UINT status;
UX_HOST_CLASS_CDC_ACM_RECEPTION cdc_acm_reception;

/* Stop background reception. The reception parameter should be the same
    that was passed to ux_host_class_cdc_acm_reception_start. */
status = ux_host_class_cdc_acm_reception_stop(cdc_acm, &cdc_acm_reception);

/* If status equals UX_SUCCESS, background reception has successfully stopped. */
```
