# Haversine Formula to get GeoPoints in X Radius



        [TestMethod]
        public void Should_UseHaversineFormula_ReturnKios()
        {
            const double latitude = 38.7344;
            const double longitude = -9.14707;
            const double radius = 11; // 11km distance
            const double earthRadiusKm = 6371;

            var result = (from dp in this._dbContext.DeliveryPoints
                          let distance = earthRadiusKm *
                          SqlFunctions.Acos(SqlFunctions.Sin(latitude * SqlFunctions.Pi() / 180) * SqlFunctions.Sin((double)dp.Latitude * SqlFunctions.Pi() / 180)
                          +
                          SqlFunctions.Cos(latitude * SqlFunctions.Pi()  / 180) * SqlFunctions.Cos((double)dp.Latitude * SqlFunctions.Pi() / 180)
                          *
                          SqlFunctions.Cos((double)dp.Longitude * SqlFunctions.Pi()  / 180 - longitude * SqlFunctions.Pi()  / 180))

                          where distance <= radius
                          select dp).ToList();

            result.Should().NotBeNull();

            result.Should().HaveCountGreaterThan(0);
        }
