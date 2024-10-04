DECLARE
    -- Global variable
    g_discount_rate CONSTANT NUMBER := 0.1; -- 10% discount
    g_total_sales NUMBER := 0; -- Total sales initialized to 0

    -- Local variables
    v_product_id NUMBER;
    v_sales_amount NUMBER;
    v_discounted_sales NUMBER;

    -- Cursor to fetch product sales data
    CURSOR c_product_sales IS
        SELECT product_id, sales_amount
        FROM products; -- Assume a products table with sales amounts

BEGIN
    -- Open cursor and loop through each product
    FOR r_product IN c_product_sales LOOP
        v_product_id := r_product.product_id;
        v_sales_amount := r_product.sales_amount;

        -- Handle NULL values by using NVL function
        v_sales_amount := NVL(v_sales_amount, 0); -- If NULL, set to 0

        -- Calculate discounted sales
        v_discounted_sales := v_sales_amount - (v_sales_amount * g_discount_rate);

        -- Accumulate total sales
        g_total_sales := g_total_sales + v_discounted_sales;

        -- Print product details and discounted sales
        DBMS_OUTPUT.PUT_LINE('Product ID: ' || v_product_id || 
                             ', Sales Amount: ' || v_sales_amount || 
                             ', Discounted Sales: ' || v_discounted_sales);
    END LOOP;

    -- Print the final total sales
    DBMS_OUTPUT.PUT_LINE('Total Sales after Discounts: ' || g_total_sales);
    
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
END;
/# create-lip
