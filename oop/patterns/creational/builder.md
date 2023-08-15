# Builder

Builder pattern is using to create similar objects with different functionality, without having unnecessary parameters.
For example, we have House class. In that class there are different methods - for adding garage, swimming pool, garden, 
second floor e.t.c., so using that House class, we can get many different houses with different parts.

<pre>
    class House
    {
        private $garage = null;
        private $swimmingPool = null;
        private $garden = null;
        private $secondFloor = false;
        private $statues = 0;

        public function __construct($garage, $swimmingPool, $garden, $secondFloor, $statues)
        {
            $this->garage = $garage;
            $this->swimmingPool = $swimmingPool;
            $this->garden = $garden;
            $this->secondFloor = $secondFloor;
            $this->statues = $statues;
        }

        //Getters

        public function getGarage()
        {
            return $this->garage;
        }

        public function getSwimmingPool()
        {
            return $this->swimmingPool;
        }

        public function getGarden()
        {
            return $this->garden;
        }

        public function getSecondFloor()
        {
            return $this->secondFloor;
        }

        public function getStatues()
        {
            return $this->statues;
        }

        //Setters

        public function setGarage($garage)
        {
            $this->garage = $garage;
        }

        public function setSwimmingPool($swimmingPool)
        {
            $this->swimmingPool = $swimmingPool;
        }

        public function setGarden($garden)
        {
            $this->garden = $garden;
        }

        public function setSecondFloor($secondFloor)
        {
            $this->secondFloor = $secondFloor;
        }

        public function setStatues($statues)
        {
            $this->statues = $statues;
        }
    }
</pre>

And now, for creating different houses, we should create objects of House in this way

<pre>
    $smallHouseWithStatues = new House(null, null, null, false, 2);
    $houseWithTwoFloors = new House(null, null, null, true, 0);
</pre>

So here we are setting many unnecessary parameters, instead of this we can use builder

<pre>
    class House
    {
        private $garage = null;
        private $swimmingPool = null;
        private $garden = null;
        private $secondFloor = false;
        private $statues = 0;

        //Getters

        public function getGarage()
        {
            return $this->garage;
        }

        public function getSwimmingPool()
        {
            return $this->swimmingPool;
        }

        public function getGarden()
        {
            return $this->garden;
        }

        public function getSecondFloor()
        {
            return $this->secondFloor;
        }

        public function getStatues()
        {
            return $this->statues;
        }

    }
</pre>

<pre>
    interface Builder
    {
        public function getResult();
    }
</pre>

<pre>
    class HouseBuilder implements Builder
    {
        private $house;

        public function __construct() 
        {
            $this->reset();
        }

        private function reset() 
        {
            $this->house = new House();
        }

        //Setters

        public function setGarage($garage)
        {
            $this->house->garage = $garage;
            return $this;
        }

        public function setSwimmingPool($swimmingPool)
        {
            $this->house->swimmingPool = $swimmingPool;
            return $this;
        }

        public function setGarden($garden)
        {
            $this->house->garden = $garden;
            return $this;
        }

        public function setSecondFloor($secondFloor)
        {
            $this->house->secondFloor = $secondFloor;
            return $this;
        }

        public function setStatues($statues)
        {
            $this->house->statues = $statues;
            return $this;
        }

        public function getResult()
        {
            $house = $this->house;
            $this->reset();
            return $house;
        }
    }
</pre>

And in this way we can create different house objects like this

<pre>
    $builder = new HouseBuilder();
    $smallHouseWithStatues = $builder->setStatues(2)->getResult();
    $houseWithTwoFloors = $builder->setSecondFloor(true)->getResult();
</pre>

Or we can use director class to create houses from patterns

<pre>
    class HouseDirector
    {
        private $builder;

        public function setBuilder(Builder $builder)
        {
            $this->builder = $builder;
        }

        public function createSmallHouseWithStatues()
        {
            return $this
                ->builder
                ->setStatues(2)
                ->getResult();
        }

        public function createHouseWithTwoFloors()
        {
            return $this
                ->builder
                ->setSecondFloor(true)
                ->getResult();
        }

        public function createBigHouse()
        {
            return $this
                ->builder
                ->setGarage('5x5')
                ->setSwimmingPool(true)
                ->setGarden('15x7')
                ->setSecondFloor(true)
                ->getResult();
        }
    }
</pre>

Which will be used like this

<pre>
    $director = new HouseDirector();
    $director->setBuilder(new HouseBuilder());

    $smallHouseWithStatues = $director->createSmallHouseWithStatues();
    $houseWithTwoFloors = $director->createHouseWithTwoFloors();
    $bigHouse = $director->createBigHouse();
</pre>