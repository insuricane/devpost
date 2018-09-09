## Inspiration

It's September 8th 2017 and Hurricane Irma is twisting her way towards Florida. Like 95% of home-owning Floridians, you have a home insurance plan. But, like most others, you don't have full flood or natural disaster coverage. No traditional insurance company will take your business with Irma on the horizon. CNN is telling you it might be the most damaging storm of the century. You wonder how you protect your most valuable asset: your home. 

With Insuricane, you can sign up for short-term home insurance using our webapp. Our servers immediately calculate the probability of damage to your home, our optimal hedging portfolio, and your initial payment.

## What it does

The basic idea of Insuricane that we can instantly hedge our risk as an insurer by taking on a position in an inversely-correlated portfolio. Most impoortantly, we can hedge the millisecond an order comes in, reducing basis risk by crafting the best hedge based on your precise location.

For example, we might short sell local utility and real estate holdings, and go long on disaster suppliees like canned foods. For PennApps, we've focused on utilities, are there are several studies displaying a correlation following hurricanes. Here's the pipeline:

1. The user inputs their address and reqested insurance coverage (in USD).
2. We use NOAA hurricane data and our model to estimate the probability of the home being destroyed by the disaster. If it is too risky, we may chose not to offer the insurance product.
3. Converting their address to latitude and longitude, use EIA data to find nearby power stations and utilities. There are 117 spread around Florida. 
4. We use a database we created to determinee the owner of the power station. If the owner is not a publicly listed company, there is no way to use it to hedge and we delete the row.
5. We use EIA data on electrical plant construction costs to estimate the $ loss to the public company if the utility were to be destroyed, and sum over all their utilities (and subsidiaries) in the affected area. This is a function of the type of plant and the capacity (in megawatts). If the loss is immaterial to the company it will not effect their stock price, and thus cannot be used to hedge. In that case we throw the row out.
6. We determine the joint probability (i.e. home destroyed and asset destroyed) by a simple heuristic: inverse squared distance to the house. 
7. Companies that have made it to this step are valid members of our heding portfolio. We weight them according to their correlation with the house (5) and how material the damage would be to the company (damage divided by market cap).
8. Having constructed a hedging portfolio and calculated the user's risk, we present the user with their premium. The premium is a lump-sum initial payment, equal to their requested insurance times the probability of impact. The user can choose to buy the insurance or reject.
9. The user signs our agreement with DocuSign.


## How we built it

The front end is built using React with Leaflet and DocuSign's API. The backend is built using Python and related data science and GIS libraries. Our data on the precise location, power plant type, and owner was pieced together by several joins on [EIA](https://www.eia.gov/) datasets. Predicted hurricane wind speed, path data, and location are derived from two [NOAA](http://www.noaa.gov/) datasets. Our focus on Hurricane Irma allowed us to trim our database to Florida, though the EIA datasets are national and could be used for any US state.

## Challenges we ran into

## Accomplishments that we're proud of

## What we learned

## What's next for Insuricane
