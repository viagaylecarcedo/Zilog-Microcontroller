#### SAMPLE CODE 

#include <ez8.h>
#include <sio.h>
#include <Stdio.h>
#define SW0 (PBIN & 0x01)
#define SW1 (PBIN & 0x02)
#define SW2 (PBIN & 0x04)
#define SW3 (PBIN & 0x08)
void main()
{
	unsigned char PrevSW0,PrevSW1,PrevSW2,PrevSW3;
	PCDD &= 0x00;
	PCAF &= 0x00;
	PCOC &= 0x00;
	PCHDE |= 0xFF;
	
	// configure the lower nibble of port B
	PBDD |= 0X0F;   // PB Data Direction     = Output
	PBAF &=~0X0F;   // PB Alternate Function = Normal
	
	// configure PORT A 
	PAAF |= 0X30;   // PA Alternate Function = UART0

	// configure UART0
    init_uart(_UART0, _DEFFREQ, _DEFBAUD);
	select_port(_UART0);
	
	// Initial values
      PAOUT   =~0x00;
      PrevSW0 = SW0;
	  PrevSW1 = SW1;
	  PrevSW2 = SW2;
	  PrevSW3 =~SW3;
	
    while(1)
	{
		   
		while((PrevSW3==SW3) && (PrevSW2==SW2) && 
		(PrevSW1==SW1) && (PrevSW0==SW0));
			
		if(SW0) // or if (SW0 !=0)
		{
			PCOUT &= ~0X00;
    		printf("  SW0 is open \t");	
		}
		
		else
		{
				
			// LED LEFT TO RIGHT
			PCOUT = ~0X10; delay_ms(200);
			PCOUT = ~0X18; delay_ms(200);
			PCOUT = ~0X1C; delay_ms(200);
			PCOUT = ~0X1E; delay_ms(200);
			PCOUT = ~0X1F; delay_ms(200);
			printf("  SW0 is closed\t");
				
		}
		
		if(SW1) // or if (SW1 !=0)
		{
			PCOUT = ~0X00;
			printf("  SW1 is open \t");

		}
		else
		{
			PCOUT = ~0X01; delay_ms(200);
			PCOUT = ~0X03; delay_ms(200);
			PCOUT = ~0X07; delay_ms(200);
			PCOUT = ~0X0F; delay_ms(200);
			PCOUT = ~0X1F; delay_ms(200);
			printf("  SW1 is closed\t");

				
		}
			
		if(SW2) // or if (SW2 !=0)
		{
			PCOUT = ~0X00;
			printf("  SW2 is open \t");
		}
		
		else
		{
			PCOUT = ~0X00; delay_ms(200);
			PCOUT = ~0X10; delay_ms(200);
			PCOUT = ~0X08; delay_ms(200);
			PCOUT = ~0X14; delay_ms(200);
			PCOUT = ~0X0A; delay_ms(200);
			PCOUT = ~0X05; delay_ms(200);
			PCOUT = ~0X02; delay_ms(200);
			PCOUT = ~0X01; delay_ms(200);
			PCOUT = ~0X00; delay_ms(200);
			printf("  SW2 is closed\t");

		}
			
		if(SW3) // or if (SW3 !=0)
		{
			PCOUT = ~0X00; 
			printf("  SW3 is open \t");
		}
		else
		{
			PCOUT = ~0X00; delay_ms(200);
			PCOUT = ~0X01; delay_ms(200);
			PCOUT = ~0X02; delay_ms(200);
			PCOUT = ~0X05; delay_ms(200);
			PCOUT = ~0X0A; delay_ms(200);
			PCOUT = ~0X14; delay_ms(200);
			PCOUT = ~0X08; delay_ms(200);
			PCOUT = ~0X10; delay_ms(200);
			PCOUT = ~0X00; delay_ms(200);
			printf("  SW3 is closed\t");
		}
			
		putch('\n');
		PrevSW0 = SW0;
	    PrevSW1 = SW1;
	    PrevSW2 = SW2;
	    PrevSW3 = SW3;
	}
}

void delay_ms(unsigned int delay)
{
	unsigned int x,y;
	for(x=0;x<=delay;x++)
		for(y=0;y<=512;y++);
}

-------
