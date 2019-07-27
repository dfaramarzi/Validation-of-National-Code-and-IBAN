# Validation-of-National-Code-and-IBAN
Check validation of Iranian National Code and IBAN in C#.

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
# IBAN Validation
```
      public bool IsValid(string IBAN)
        {
            IBAN = IBAN.ToUpper();
            if (IBAN.Length != 26) return false;
            var changediban = IBAN.Substring(4, 22);
            changediban = changediban.Insert(22, (Convert.ToInt16(IBAN[0]) - 55).ToString());
            changediban = changediban.Insert(24, (Convert.ToInt16(IBAN[1]) - 55).ToString());
            changediban = changediban.Insert(26, IBAN.Substring(2, 2));
            if (BigInteger.Parse(changediban) % 97 != 1) return false;
            return true;
        }
```
