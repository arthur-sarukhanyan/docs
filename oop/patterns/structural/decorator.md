# Decorator

Decorator pattern is using for add an extra functionality to objects dynamically, which will be stored in wrappers called
decorators. We have class House with get description and price methods.

<pre>
    interface House
    {
        public function description();

        public function price();
    }
</pre>

<pre>
    class BasicHouse implements House
    {
        public function description()
        {
            return 'Basic House';
        }

        public function price()
        {
            return 15000;
        }
    }
</pre>

In this case decorator will help us to get description and price of different houses.

<pre>
    abstract class SpecificHouse implements House
    {
        private $house;

        public function __construct(House $house)
        {
            $this->house = $house;
        }

        abstract function description();

        abstract function price();
    }
</pre>

<pre>
    class HouseWithGarden extends SpecificHouse
    {
        public function description()
        {
            return $this->house->description() . ' with garden';
        }

        public function price()
        {
            return $this->house->price() + 5000;
        }
    }
</pre>

<pre>
    class HouseWithSwimmingPool extends SpecificHouse
    {
        public function description()
        {
            return $this->house->description() . ' with swimming pool';
        }

        public function price()
        {
            return $this->house->price() + 2000;
        }
    }
</pre>

So now we have different options of houses, and this is how it could be used

<pre>
    $basicHouse = new BasicHouse();
    $houseWithGarden = new HouseWithGarden($basicHouse);
    $houseWithGardenAndSwimmingPool = new HouseWithSwimmingPool($houseWithGarden);
</pre>