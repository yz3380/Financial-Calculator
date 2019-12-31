# Financial-Calculator
Term Project for COMS3101 Python Data Structure

### Author
yz3380@columbia.edu

This is a simple financial calculator for bond and option pricing.    
The calculator is built on tkinter class pages and contains two parts:  

- **Bond Pricing**  
- **European put / call option pricing**  

Can switch pages between index page, bond pricing page and option pricing page.  
Previous page will be closed automatically.  

## Bond Pricing

The bond pricing model is based on simple [bond valuation](https://en.wikipedia.org/wiki/Bond_valuation).
The pricing formula is as below:

<img style="float: left;" src="https://wikimedia.org/api/rest_v1/media/math/render/svg/b1ad41f50e722bb5c731bc1014f6ac4caedd14bd">
<br clear="all" />

where:

- F = face value or par values (usually set to \$1000)  
- C = F * coupon rate = coupon payment (periodic interest payment)  
- N = T * n = number of payments( years to maturity devided by payment frequency)  
- i = market interest rate, or required yield, or observed / appropriate yield to maturity (Y/I)  
- M = value at maturity, usually equals face value  
- P = market price of bond.  

Here we have 4 parameters P, c, I/Y and T and one selection of payment frequency (n).  
Using numpy financial cash flow functions, we can get any one of these values after knowing the other 3 as inputs.  
The valid space for parameters are:
- P : must be positive  
- c : greater or equal to zero, when c = 0 it is a zero coupon bond and P = F*(1+i)^(-N) where N = T * n
- I/Y : must be positive and write as annual rate in percentage
- T : must be positive  

If the inputs are invalid, the program will automatically handle the error and pop up a error messagebox.  
If you enter all the parameters, the program will modify the coupon payment by default.  
There might be some constraints in the numpy functions so they are deployed in try/except format too.  

### Sample Output:

Set coupon = 80\$;  
Set yield to maturity = 8\%;  
Set years to maturity = 2;  
Select Annualy payment and leave the Price blank;  
Press Calculate button and the program will automatically calculate the price of the bond equals to \$1000.   
In fact, since annual coupon rate equals to market rate, the bond is always priced at par (default \$1000), no matter the years.  
The interpretation is followed as:  
"A \$1000 face value bond with annual coupon rate 8.0%, paid annually, matures in 2.0 years,
is priced at \$1000.0 at the initial date given the annual interest rate is 8.0\%."  


## Option Pricing

The option pricing model can only calculate price for plain [European put](https://en.wikipedia.org/wiki/Option_style) / [call options](https://en.wikipedia.org/wiki/Call_option).  
- European call option payoff = max{(S - K), 0} at maturity date
- European put option payoff = max{(K - S), 0} at maturity date  

The price is equal to payoff discounted to its current value.  
We assume the stock price follows a [Geometric Brownian motion](https://en.wikipedia.org/wiki/Geometric_Brownian_motion) and simulate option price using [Monte Carlo method](https://en.wikipedia.org/wiki/Monte_Carlo_method).  

That is:

<img style="float: left;" src="https://wikimedia.org/api/rest_v1/media/math/render/svg/266807b65fd50635526a766c0c89a2913085d0c2">
<br clear="all" />

where $S_{0}$ is the current stock price, $S_{t}$ is the stock price at maturity,  
$W_{t}$ is a [Wiener process](https://en.wikipedia.org/wiki/Wiener_process) or Brownian motion, and $\mu$  ("the percentage drift") and $\sigma$  ("the percentage volatility") are constants.  

### Simulation steps:
1.Set all inputs be positive, and volatility between 0 and 1;  
2.Generate N paths of stock price $$S_{t}$$, then calculate the option payoff;  
3.Calculate the mean payoff of all paths, discounted to current value;  
4.Simulated option price = current value of mean payoff.  

The simulation result could be verified by [Black Scholes formula](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_model).

In fact the we can get the option price directly by applying the Black Scholes formula.  
However, when adding some barriers or restrictions to an option, or the option is even more complex that it will depend on 2 or more stocks,  
it is easy to modify the simulation algorithms and get the estimated price which can not be derived by Black Scholes formula explicitly.  
This program is just a basic model with simple user interface.  

### Sample Output:  

Set S0 = 100;  
Set annual interest rate r = 5\%;  
Set volatility $\sigma$ = 0.2;  
Set T = 0.5 years;  
Set strike price K = 100;  
Set any number of paths N >100 (to make the estimatation more accurate);  
Choose option type as call option;  
Run the Get Price button;   
The theoretical price is \$6.889 and simulation result plot is printed in notebook as well. 
