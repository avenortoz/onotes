To enable [[JWT]] authentication we need:
1. To add `AddAuthentication` service schema with `(JwtBearerDefaults.AuthenticationScheme)` parameter which is a string `Bearer`
2. To add `AddJwtBearer` service
3. To add `UseAuthentication` middleware
4. To add `UseAuthorization` middleware
5. After that can be added other middlewares which need authorization

## Snippets:
### How to add [[JWT]] authorization with authentication to the asp.net project
```csharp
builder.Services.AddAuthorization();
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidIssuer = AuthOptions.ISSUER,
            ValidateAudience = true,
            ValidAudience = AuthOptions.AUDIENCE,
            ValidateLifetime = true,
            IssuerSigningKey = AuthOptions.GetSymmetricSecurityKey(),
            ValidateIssuerSigningKey = true,
        };
    });
```
### How to generate [[JWT]] token
```
    var claims = new List<Claim> { new Claim("the_secret_name", username) };
    // создаем JWT-токен
    var jwt = new JwtSecurityToken(
            issuer: AuthOptions.ISSUER,
            audience: AuthOptions.AUDIENCE,
            claims: claims,
            expires: DateTime.UtcNow.Add(TimeSpan.FromMinutes(2)),
            signingCredentials: new SigningCredentials(AuthOptions.GetSymmetricSecurityKey(), SecurityAlgorithms.HmacSha256));

    return new JwtSecurityTokenHandler().WriteToken(jwt);
```
### How to read [[JWT]] token
- Using directly HTTPContext
```
    string? token = httpContext.Request.Headers["Authorization"].FirstOrDefault()?.Split(" ").Last();
    var tokenHandler = new JwtSecurityTokenHandler();
    tokenHandler.ValidateToken(token, new TokenValidationParameters
    {
        ValidateIssuerSigningKey = true,
        IssuerSigningKey = AuthOptions.GetSymmetricSecurityKey(),
        ValidateIssuer = false,
        ValidateAudience = false,
        ClockSkew = TimeSpan.Zero
    }, out SecurityToken validatedToken);
    var jwtToken = (JwtSecurityToken) validatedToken;
    var secretName = jwtToken.Claims.First(x => x.Type == "the_secret_name").Value; 
```
- Using `ClaimsPrincipal`
```
var secretName = httpContext.User.FindFirst("the_secret_name")?.Value;
logger.LogInformation(secretName);
return new { message = "Hello World!" };
```
