Consommation non sécurisée des API (API10:2023)
En raison de la vulnérabilité ci-dessus, l'attaquant est capable d'envoyer ou de recevoir des informations à partir des sources de la chaîne d'approvisionnement ou d'implémenter ses demandes souhaitées dans un groupe spécifique.

Exemple
GET demande de recevoir des informations météorologiques d'un service tiers:

GET /api/weather?location=New+York
Code non conforme (.NET)
[ApiController]
[Route("api/weather")]
public class WeatherController : ControllerBase
{
    private readonly IWeatherService weatherService;
    public WeatherController(IWeatherService weatherService)
    {
        this.weatherService = weatherService;
    }

    // GET /api/weather
    [HttpGet]
    public IActionResult GetWeather(string location)
    {
        // Make a direct call to the third-party weather API
        WeatherData weatherData =
        weatherService.GetWeatherData(location);

        return Ok(weatherData);
    }
    
    // Other methods...
}
Code conforme (.NET)
[ApiController]
[Route("api/weather")]
public class WeatherController : ControllerBase
{
    private readonly IWeatherService weatherService;
    public WeatherController(IWeatherService weatherService)
    {
        this.weatherService = weatherService;
    }

    // GET /api/weather
    [HttpGet]
    public IActionResult GetWeather(string location)
    {
        // Validate the location parameter and restrict access to trusted sources
        if (!IsValidLocation(location))
        {
         return BadRequest();
        }

        // Make a call to the third-party weather API through the weather service
        WeatherData weatherData = weatherService.GetWeatherData(location);

        if (weatherData == null)
        {
            return NotFound();
        }
        return Ok(weatherData);
    }
    private bool IsValidLocation(string location)
    {
        // Implement validation logic to ensure the location is safe and trusted

        // This could involve white-listing trusted sources or validating against a known set of safe locations

        // Return true if the location is valid, false otherwise
        // Example: return Regex.IsMatch(location, "^[a-zA-Z]+(,[a-zA-Z]+)*$");

        // Implement your validation logic here
        // For simplicity, assuming any location is valid
        return true;
    }

    // Other methods...
}
Suggestions générales de prévention:
Faites confiance aux données reçues des API externes avec prudence et validation rigoureuse.

Vérifiez et vérifiez la sécurité et les normes du service tiers avant de vous y connecter.

Utiliser le cryptage pour communiquer avec des services externes et empêcher l'envoi d'informations sensibles normalement.

Limiter l'accès et les niveaux autorisés aux services tiers et fixer des limites appropriées.

Mettre en œuvre des mécanismes de protection tels que le prototypage et la généralisation pour assurer la sécurité et la fiabilité des données reçues de services externes.

Surveillance et surveillance continues pour détecter et corriger tout défaut dans la sécurité des services externes.

Former les développeurs aux principes de sécurité et à l'utilisation correcte des API externes.










































































