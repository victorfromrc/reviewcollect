# PrestaShop

Connect your PrestaShop store to Review Collect to trigger your review requests automatically from your orders.

## Create your PrestaShop Webservice key

1. Log in to your PrestaShop Back Office.
2. In the left menu, click **Advanced Parameters**, then **Webservice**.
3. If not already done, enable the Webservice by selecting **Yes** for "Enable PrestaShop's webservice", then save.
4. Click **Add new webservice key**, then **Generate** (or enter your own key) and give it a name, for example "Review Collect".
5. Grant read permissions (GET/VIEW) on these resources: addresses, countries, customers, languages, orders, order_histories, order_states, order_details, carriers and products.
6. Click **Save** to store the key.

## Authorize Review Collect's IP addresses

So that Review Collect can reach your Webservice, authorize its IP addresses on your PrestaShop. The current list is in the help center article, which is kept up to date: https://help.review-collect.com/en/article/prestashop-integration-1mncw9g/

## Connect your store to Review Collect

1. Go to the **Integrations** page in Review Collect.
2. Click **PrestaShop**, then **Connect**.
3. Enter your Webservice key, your store URL and the requested information.

## Show product reviews on your store

On PrestaShop, you add the star badge, the reviews list and the all reviews page by pasting the code into your theme. See the help center for the code and the steps.

## Related

- [Collect reviews](../collect.md)
- [Showcase reviews](../showcase.md)
