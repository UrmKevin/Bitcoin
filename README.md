# Bitcoin
using Newtonsoft.Json;
using System;
using System.IO;
using System.Net;

namespace BitCoin_CAL
{
    class Program
    {
        static void Main(string[] args)
        {
            BitCoinRate bitcoin = GetRates();

            Console.WriteLine($"Current rate in {bitcoin.bpi.USD.code}: {bitcoin.bpi.USD.rate_float}");
            Console.WriteLine($"Current rate in {bitcoin.bpi.EUR.code}: {bitcoin.bpi.EUR.rate_float}");
            Console.WriteLine($"Current rate in {bitcoin.bpi.GBP.code}: {bitcoin.bpi.GBP.rate_float}");
        }


        public static BitCoinRate GetRates()
        {
            string url = "https://api.coindesk.com/v1/bpi/currentprice.json";
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);
            request.Method = "GET";

            var webResponse = request.GetResponse();
            var webStream = webResponse.GetResponseStream();

            BitCoinRate bitcoin;

            using (var responseReader = new StreamReader(webStream))
            {
                var response = responseReader.ReadToEnd();
                bitcoin = JsonConvert.DeserializeObject<BitCoinRate>(response);
            }

            return bitcoin;

        }
    }
}





using System;
using System.Collections.Generic;
using System.Text;

namespace BitCoin_CAL
{
    class BitCoinRate
    {
        public bpi bpi { get; set; }
    }


    public class bpi
    {
        public USD USD { get; set; }
        public EUR EUR { get; set; }
        public GBP GBP { get; set; }

    }
    public class USD
    {
        public string code { get; set; }
        public float rate_float { get; set; }
        
    }
    public class EUR
    {
        public string code { get; set; }
        public float rate_float { get; set; }

    }
    public class GBP
    {
        public string code { get; set; }
        public float rate_float { get; set; }

    }

}
