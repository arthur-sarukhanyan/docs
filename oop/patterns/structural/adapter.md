# Adapter

 Adapter pattern is using for adapting some class/object to some general functionality. For example if we have a class, which should not be modified, 
but we want to add some interface to it. So if we have two payment classes, implementing the same interface, and we have third payment 
class, which was written in some different way and doesn't have that interface method/methods, but we want to use it as the other payment
methods, we can use adapter pattern to solve this problem. And it could be done in 2 ways. So we have this classes

<pre>
    interface Payment 
    {
        public function pay();
    }
</pre>

<pre>
    class Stripe implements Payment 
    {
        public function pay()
        {
            //pay by Stripe
        }
    }
</pre>

<pre>
    class PayPal implements Payment 
    {
        public function pay()
        {
            //pay by PayPal
        }
    }
</pre>

And also class Mollie, which doesn't have pay method, but processPayment method instead

<pre>
    class Mollie
    {
        public function processPayment()
        {
            //pay by Mollie
        }
    }
</pre>

First way to solve this problem is to have additional class, which will use an object of Mollie

<pre>
    class MollieAdapter implements Payment
    {
        public function pay()
        {
            $mollie = new Mollie();
            $mollie->processPayment();
        }
    }
</pre>

And the second way, which is better solution, is to also have separate class, but which will extend from Mollie class

<pre>
    class MollieAdapter extends Mollie implements Payment
    {
        public function pay()
        {
            $this->processPayment();
        }
    }
</pre>