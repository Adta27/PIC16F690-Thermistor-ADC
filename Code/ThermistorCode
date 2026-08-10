include <P16F690.inc>
        __config (_INTRC_OSC_NOCLKOUT & _WDT_OFF & _PWRTE_OFF & _MCLRE_OFF & _CP_OFF & _BOR_OFF & _IESO_OFF & _FCMEN_OFF)

; PIC16F690 Thermometer
; Thermistor input: RB4 / AN10 / physical pin 13
;STUDENT: ADITYA DUTTA
;TEACHER: Mr. VANHOUT
;DATE: 2026-06-15	
;
; TRUE PORTC LED PIN MAP (Bits: 76543210):
; RC7 (Pin 9)  - LED 4 (Yellow)  
; RC6 (Pin 8)  - LED 5 (Green)
; RC5 (Pin 5)  - LED 8 (Blue)
; RC4 (Pin 6)  - LED 7 (Blue)
; RC3 (Pin 7)  - LED 6 (Green)   
; RC2 (Pin 14) - LED 3 (Yellow)
; RC1 (Pin 15) - LED 2 (Red)
; RC0 (Pin 16) - LED 1 (Red)
;
; Higher ADC = colder
; Lower ADC  = hotter

        cblock 0x20
            pot
            delay1
            delay2
            lockState   ; 0 = normal regular reading, 1 = locked state
        endc

        org 0

main:
        banksel ANSEL
        clrf    ANSEL

        banksel ANSELH
        movlw   b'00000100'     
        movwf   ANSELH

        banksel TRISC
        clrf    TRISC          

        banksel TRISB
        movlw   b'00110000'     
        movwf   TRISB

        banksel PORTC
        clrf    PORTC

        banksel lockState
        clrf    lockState

        banksel ADCON0
        movlw   b'00101001'     
        movwf   ADCON0

        banksel ADCON1
        movlw   b'00010000'     
        movwf   ADCON1

loop:
        banksel PORTB
        btfss   PORTB, 5
        goto    checkLock

        banksel lockState
        movlw   0x01
        xorwf   lockState, f

debounceWait:
        banksel PORTB
        btfsc   PORTB, 5
        goto    debounceWait
        call    delay

checkLock:
        call    readADC ; Read the current temperature from the thermistor
        
        banksel lockState
        movf    lockState, w
        sublw   .1
        btfsc   STATUS, Z
        goto    skipDisplay

        call    displayTemp ; Decide which LEDs to turn on based on the value

skipDisplay:
        call    delay ; Wait a moment so the display is stable
        goto    loop ; repeat forever

readADC:
        call    shortDelay ; this delay is different from the loop delay

        banksel ADCON0
        bsf     ADCON0, 1       ; start ADC conversion

waitADC:
        btfsc   ADCON0, 1       ; wait until done
        goto    waitADC

        banksel ADRESH
        movf    ADRESH, w

        banksel pot
        movwf   pot

        return

displayTemp:
        banksel pot

        ; pot >= 135 = coldest, LED8 only
        movlw   .135
        subwf   pot, w ; w = pot - 135(the threshold)
        btfsc   STATUS, C ; Checks if pot is greater than 135
        goto    showBlue1 ; if pot is greater than go to the turn on blue command

        ; pot >= 130 = LED8 + LED7
        movlw   .130
        subwf   pot, w
        btfsc   STATUS, C
        goto    showBlue2

        ; pot >= 125 = LED8 -> LED6
        movlw   .125
        subwf   pot, w
        btfsc   STATUS, C
        goto    showBlueGreen1

        ; pot >= 120 = LED8 -> LED5
        movlw   .120
        subwf   pot, w
        btfsc   STATUS, C
        goto    showBlueGreen2

        ; pot >= 115 = LED8 -> LED4
        movlw   .115
        subwf   pot, w
        btfsc   STATUS, C
        goto    showBlueGreenYellow1

        ; pot >= 110 = LED8 -> LED3
        movlw   .110
        subwf   pot, w
        btfsc   STATUS, C
        goto    showBlueGreenYellow2

        ; pot >= 105 = LED8 -> LED2
        movlw   .105
        subwf   pot, w
        btfsc   STATUS, C
        goto    showAlmostAll

        goto    showAll

showBlue1:
        movlw   b'00100000'     ; LED 8 (RC5)
        goto    writeLEDs

showBlue2:
        movlw   b'00110000'     ; + LED 7 (RC4)
        goto    writeLEDs

showBlueGreen1:
        movlw   b'00111000'     ; + LED 6 (RC3)
        goto    writeLEDs

showBlueGreen2:
        movlw   b'01111000'     ; + LED 5 (RC6)
        goto    writeLEDs

showBlueGreenYellow1:
        movlw   b'11111000'     ; + LED 4 (RC7)
        goto    writeLEDs

showBlueGreenYellow2:
        movlw   b'11111100'     ; + LED 3 (RC2)
        goto    writeLEDs

showAlmostAll:
        movlw   b'11111110'     ; + LED 2 (RC1)
        goto    writeLEDs

showAll:
        movlw   b'11111111'     ; All LEDs on (including LED 1 on RC0)
        goto    writeLEDs

writeLEDs:
        banksel PORTC
        movwf   PORTC
        return

shortDelay:
        banksel delay1
        movlw   .100
        movwf   delay1

shortLoop:
        decfsz  delay1, f
        goto    shortLoop
        return

delay:
        banksel delay2
        movlw   .100
        movwf   delay2

delayOuter:
        movlw   .250
        movwf   delay1

delayInner:
        nop
        decfsz  delay1, f
        goto    delayInner

        decfsz  delay2, f
        goto    delayOuter
        return

        end