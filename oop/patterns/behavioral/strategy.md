# Strategy

 Strategy pattern is using for separating similar logic from main class in the way, that it will be possible to replace them in the logic dynamically. 
 Also in that way such classes will be easier to modify. The main class will work as an interface, for working with different, but similar logic. Ex. we have 
 PaymentProcessor class, which is working with different payments, but it's doing the same action for each payment.
 
<pre>
    class PaymentProcessor
    {
        private $paymentService;

        public function processPayment()
        {
            if ($this->paymentService instanceof PayPal) {
                $paymentService->process();
            } elseif ($this->paymentService instanceof Stripe) {
                $paymentService->runPayment();
            } elseif ($this->paymentService instanceof Mollie) {
                $paymentService->pay();
            } else {
                throw new \Exception('Invalid payment service');
            }
        }

        public function setPaymentService($paymentService)
        {
            $this->paymentService = $paymentService;
        }
    }
</pre>

<pre>
    class PayPal
    {
        public function process()
        {
            //pay by PayPal
        }
    }
</pre>

<pre>
    class Stripe
    {
        public function runPayment()
        {
            //pay by Stripe
        }
    }
</pre>

<pre>
    class Mollie
    {
        public function pay()
        {
            //pay by Mollie
        }
    }
</pre>

The problem is, that in case of new payment methods, we should modify processPayment method, which violates Open-Closed Principle,
and also for many payment methods it will not look good. And this is how it will be structured in case of Strategy pattern. First
of all we need an interface for our strategies

<pre>
    interface PaymentStrategy
    {
        public function processPayment();
    }
</pre>

And this is our strategies

<pre>
    class PayPalPaymentStrategy implements PaymentsStrategy
    {
        public function processPayment()
        {
            $this->process();
        }
    }
</pre>

<pre>
    class StripePaymentStrategy implements PaymentsStrategy
    {
        public function processPayment()
        {
            $this->runPayment();
        }
    }
</pre>

<pre>
    class MolliePaymentStrategy implements PaymentsStrategy
    {
        public function processPayment()
        {
            $this->pay();
        }
    }
</pre>

And all of them will work from main PaymentProcessor class

<pre>
    class PaymentProcessor
    {
        private PaymentsStrategy $paymentStrategy;

        public function __construct(PaymentsStrategy $paymentStrategy)
        {
            $this->paymentStrategy = $paymentStrategy;
        }

        public function processPayment()
        {
            $this->paymentStrategy->processPayment();
        }
    }
</pre>