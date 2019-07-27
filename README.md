# Validation-of-National-Code-and-IBAN-In-C-
Check validation for National Code and IBAN for Iranian

# National Code Validation
```
      public bool IsValid(string socialSecurityCode)
        {
            if (string.IsNullOrEmpty(socialSecurityCode))
                return false;

            if (!new Regex(@"\d{10}").IsMatch(socialSecurityCode))
                return false;

            var array = socialSecurityCode.ToCharArray();
            var allDigitEqual = new[]
            {
                "0000000000", "1111111111", "2222222222", "3333333333", "4444444444", "5555555555", "6666666666",
                "7777777777", "8888888888", "9999999999"
            };
            if (allDigitEqual.Contains(socialSecurityCode))
                return false;

            var j = 10;
            long sum = 0;
            for (var i = 0; i < array.Length - 1; i++)
            {
                sum += long.Parse(array[i].ToString(CultureInfo.InvariantCulture)) * j;
                j--;
            }

            var div = sum / 11;
            var r = div * 11;
            var diff = Math.Abs(sum - r);
            if (diff <= 2) return diff == long.Parse(array[9].ToString(CultureInfo.InvariantCulture));
            var temp = Math.Abs(diff - 11);
            var o = long.Parse(array[9].ToString(CultureInfo.InvariantCulture));
            return temp == long.Parse(array[9].ToString(CultureInfo.InvariantCulture));
        }
        ```
