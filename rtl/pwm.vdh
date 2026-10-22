library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;

entity Papilio_Blinky is
    Port ( 
        clk     : in  STD_LOGIC;  -- Connects to the on-board 32MHz oscillator
        led_pin : out STD_LOGIC   -- Connects to your designated Wing header pin
    );
end Papilio_Blinky;

architecture Behavioral of Papilio_Blinky is
    -- 32MHz clock counter to create a slow timing tick
    signal clk_divider : unsigned(15 downto 0) := (others => '0');
    signal tick        : std_logic := '0';

    -- PWM control signals
    signal pwm_counter : unsigned(7 downto 0) := (others => '0');
    signal duty_cycle  : unsigned(7 downto 0) := (others => '0');
    signal fading_up   : boolean := true;
begin

    -- Clock divider to slow down operations for the brightness transitions
    process(clk)
    begin
        if rising_edge(clk) then
            clk_divider <= clk_divider + 1;
            if clk_divider = x"FFFF" then
                tick <= '1';
            else
                tick <= '0';
            end if;
        end if;
    end process;

    -- Adjust the duty cycle (brightness) over time
    process(clk)
    begin
        if rising_edge(clk) then
            if tick = '1' then
                if fading_up then
                    if duty_cycle = x"FF" then
                        fading_up <= false;
                    else
                        duty_cycle <= duty_cycle + 1;
                    end if;
                else
                    if duty_cycle = x"00" then
                        fading_up <= true;
                    else
                        duty_cycle <= duty_cycle - 1;
                    end if;
                end if;
            end if;
        end if;
    end process;

    -- Generate the high-frequency PWM signal to drive the LED pin
    process(clk)
    begin
        if rising_edge(clk) then
            pwm_counter <= pwm_counter + 1;
            if pwm_counter < duty_cycle then
                led_pin <= '1';
            else
                led_pin <= '0';
            end if;
        end if;
    end process;

end Behavioral;

