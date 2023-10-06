<h1>Middleware</h1>

```
<?php

namespace App\Http\Middleware;

use Closure;
use JWTAuth;
use Exception;

class JwtMiddleware
{
    public function handle($request, Closure $next)
    {
        try {
            $user = JWTAuth::parseToken()->authenticate();
        } catch (Exception $e) {
            if ($e instanceof \Tymon\JWTAuth\Exceptions\TokenInvalidException) {
                return response()->json(['message' => 'Invalid token'], 401);
            } else if ($e instanceof \Tymon\JWTAuth\Exceptions\TokenExpiredException) {
                return response()->json(['message' => 'Token has expired'], 401);
            } else {
                return response()->json(['message' => 'Token not found'], 401);
            }
        }

        // You can access the authenticated user using $user if needed.

        return $next($request);
    }
}

```
