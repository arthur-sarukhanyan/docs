# Singleton

 Singleton pattern is for having only one object of some class. This is the one of most popular realizations
 
<pre>
    class Singleton
    {
        private static $instance = null;

        private function __construct()
        {
        }

        public static function getInstance()
        {
            if (!self::$instance) {
                self::$instance = new Singleton();
            }

            return self::$instance;
        }
    }
</pre>

So object of class Singleton should always be got by calling static method getInstance 

<pre>
    $singletonInstance = Singleton::getInstance();
</pre>