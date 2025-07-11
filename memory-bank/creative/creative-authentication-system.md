# íž¨ CREATIVE PHASE: Authentication System Design

## íž¨íž¨íž¨ ENTERING CREATIVE PHASE: AUTHENTICATION SYSTEM íž¨íž¨íž¨

**Date:** $(date)  
**Phase:** 7 of 8 - Authentication System Design  
**Type:** Security Architecture Design  
**Focus:** JWT-based authentication, role-based access control, and multi-factor authentication

## PROBLEM STATEMENT

Design a comprehensive authentication system for UniNest that supports:
1. **Multi-role authentication** (Student, Landlord, University Official, Admin)
2. **JWT-based token management** with refresh token rotation
3. **Role-based access control (RBAC)** with granular permissions
4. **Multi-factor authentication (MFA)** for enhanced security
5. **Social login integration** (Google, Facebook, Apple)
6. **Password security** with bcrypt hashing and policy enforcement
7. **Session management** with device tracking and remote logout
8. **Security monitoring** with audit logs and threat detection

**Key Challenges:**
- Complex role hierarchy with university-specific permissions
- Secure token management with automatic refresh and rotation
- Multi-platform authentication (web, mobile, admin)
- Integration with university identity systems
- Compliance with data protection regulations
- Scalable session management across multiple servers
- Real-time security monitoring and threat detection
- User experience optimization while maintaining security

## OPTIONS ANALYSIS

### Option 1: Simple JWT Authentication with Basic RBAC
**Description**: Basic JWT implementation with simple role-based access control
**Pros**:
- Simple implementation and easy to understand
- Fast development and deployment
- Low complexity for team maintenance
- Standard JWT libraries available
- Good performance characteristics
**Cons**:
- Limited security features
- No advanced session management
- Basic role system without granular permissions
- No MFA support
- Limited audit and monitoring capabilities
**Complexity**: Low
**Implementation Time**: 3-4 weeks

### Option 2: Advanced JWT with Comprehensive RBAC and MFA
**Description**: Enhanced JWT system with granular permissions, MFA, and security monitoring
**Pros**:
- Comprehensive security features
- Granular permission control
- Multi-factor authentication support
- Advanced session management
- Good audit and monitoring capabilities
- Scalable architecture
**Cons**:
- More complex implementation
- Higher development and maintenance cost
- Requires security expertise
- More infrastructure requirements
- Potential performance overhead
**Complexity**: High
**Implementation Time**: 8-10 weeks

### Option 3: Hybrid Authentication with External Identity Providers
**Description**: JWT-based system with university SSO integration and social login
**Pros**:
- Best security with external identity providers
- Seamless university integration
- Reduced password management overhead
- Enhanced user experience
- Compliance with institutional policies
**Cons**:
- Most complex implementation
- Dependency on external systems
- Higher integration complexity
- More difficult to customize
- Potential vendor lock-in
**Complexity**: High
**Implementation Time**: 10-12 weeks

## DECISION

**Selected Approach**: **Option 2 - Advanced JWT with Comprehensive RBAC and MFA**

**Rationale**:
1. **MVP Requirements**: Balances security with implementation complexity
2. **University Integration**: Can integrate with university systems later
3. **Security**: Comprehensive security features for sensitive data
4. **Scalability**: Architecture supports future growth
5. **Compliance**: Meets data protection requirements
6. **User Experience**: Good balance of security and usability

## IMPLEMENTATION PLAN

### Phase 1: Core JWT Authentication (Week 1-2)
```
JWT Implementation
âââ Access token generation and validation
âââ Refresh token rotation
âââ Token blacklisting
âââ Password hashing with bcrypt
âââ Basic user registration and login

Database Schema
âââ users table with authentication fields
âââ refresh_tokens table for token management
âââ user_sessions table for session tracking
âââ audit_logs table for security monitoring
```

### Phase 2: Role-Based Access Control (Week 3-4)
```
RBAC Implementation
âââ Role hierarchy definition
âââ Permission system design
âââ Role assignment and management
âââ Permission checking middleware
âââ Dynamic permission loading

University Integration
âââ University-specific roles
âââ Department-based permissions
âââ Course-based access control
âââ Exam period permissions
```

### Phase 3: Multi-Factor Authentication (Week 5-6)
```
MFA Implementation
âââ TOTP (Time-based One-Time Password)
âââ SMS-based verification
âââ Email-based verification
âââ Backup codes generation
âââ MFA enrollment and management

Security Features
âââ Device fingerprinting
âââ Location-based authentication
âââ Risk-based authentication
âââ Account lockout protection
```

### Phase 4: Advanced Security and Monitoring (Week 7-8)
```
Security Monitoring
âââ Real-time threat detection
âââ Anomaly detection algorithms
âââ Security audit logging
âââ Automated response systems
âââ Security dashboard

Session Management
âââ Multi-device session tracking
âââ Remote logout capabilities
âââ Session timeout management
âââ Device trust management
```

## VISUALIZATION

### Authentication System Architecture
```
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                        CLIENT LAYER                             â
â  âââââââââââââââââââ  âââââââââââââââââââ  âââââââââââââââââââ  â
â  â   Web Client    â  â  Mobile Client  â  â  Admin Client   â  â
â  â   (React)       â  â   (Android)     â  â   (Dashboard)   â  â
â  âââââââââââââââââââ  âââââââââââââââââââ  âââââââââââââââââââ  â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
                                â
                      âââââââââââźââââââââââ
                      â   API GATEWAY     â
                      â   (Rate Limiting) â
                      âââââââââââŹââââââââââ
                                â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                    AUTHENTICATION LAYER                         â
â âââââââââââââââ âââââââââââââââ âââââââââââââââ âââââââââââââââ â
â â   JWT Auth  â â   RBAC      â â   MFA       â â   Session   â â
â â   Service   â â   Service   â â   Service   â â   Manager   â â
â âââââââââââââââ âââââââââââââââ âââââââââââââââ âââââââââââââââ â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
                                â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                      SECURITY LAYER                             â
â âââââââââââââââââââ âââââââââââââââââââ âââââââââââââââââââââââ â
â â   Threat        â â   Audit         â â   Device            â â
â â   Detection     â â   Logging       â â   Management        â â
â âââââââââââââââââââ âââââââââââââââââââ âââââââââââââââââââââââ â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
```

### Authentication Flow Diagram
```
User Login Flow
âââ User enters credentials
âââ Validate credentials
âââ Check MFA requirement
â   âââ If MFA enabled: Send verification code
â   âââ If MFA disabled: Proceed to token generation
âââ Generate access and refresh tokens
âââ Store refresh token securely
âââ Return access token to client
âââ Log authentication event

Token Refresh Flow
âââ Client sends refresh token
âââ Validate refresh token
âââ Check token blacklist
âââ Generate new access token
âââ Rotate refresh token
âââ Blacklist old refresh token
âââ Return new tokens
```

## JWT IMPLEMENTATION

### Token Management Service
```typescript
import jwt from 'jsonwebtoken';
import bcrypt from 'bcrypt';
import crypto from 'crypto';

interface TokenPayload {
  userId: string;
  email: string;
  role: string;
  permissions: string[];
  universityId?: string;
  departmentId?: string;
}

class JWTService {
  private readonly ACCESS_TOKEN_SECRET = process.env.JWT_ACCESS_SECRET;
  private readonly REFRESH_TOKEN_SECRET = process.env.JWT_REFRESH_SECRET;
  private readonly ACCESS_TOKEN_EXPIRY = '15m';
  private readonly REFRESH_TOKEN_EXPIRY = '7d';
  
  async generateTokens(user: User): Promise<{ accessToken: string; refreshToken: string }> {
    const payload: TokenPayload = {
      userId: user.id,
      email: user.email,
      role: user.role,
      permissions: await this.getUserPermissions(user.id),
      universityId: user.universityId,
      departmentId: user.departmentId
    };
    
    const accessToken = jwt.sign(payload, this.ACCESS_TOKEN_SECRET, {
      expiresIn: this.ACCESS_TOKEN_EXPIRY,
      issuer: 'uninest',
      audience: 'uninest-users'
    });
    
    const refreshToken = jwt.sign(
      { userId: user.id, tokenId: crypto.randomUUID() },
      this.REFRESH_TOKEN_SECRET,
      {
        expiresIn: this.REFRESH_TOKEN_EXPIRY,
        issuer: 'uninest',
        audience: 'uninest-refresh'
      }
    );
    
    // Store refresh token in database
    await this.storeRefreshToken(user.id, refreshToken);
    
    return { accessToken, refreshToken };
  }
  
  async verifyAccessToken(token: string): Promise<TokenPayload> {
    try {
      const decoded = jwt.verify(token, this.ACCESS_TOKEN_SECRET, {
        issuer: 'uninest',
        audience: 'uninest-users'
      }) as TokenPayload;
      
      // Check if token is blacklisted
      const isBlacklisted = await this.isTokenBlacklisted(token);
      if (isBlacklisted) {
        throw new Error('Token is blacklisted');
      }
      
      return decoded;
    } catch (error) {
      throw new Error('Invalid access token');
    }
  }
  
  async refreshAccessToken(refreshToken: string): Promise<{ accessToken: string; refreshToken: string }> {
    try {
      const decoded = jwt.verify(refreshToken, this.REFRESH_TOKEN_SECRET, {
        issuer: 'uninest',
        audience: 'uninest-refresh'
      }) as { userId: string; tokenId: string };
      
      // Verify refresh token exists in database
      const storedToken = await this.getRefreshToken(decoded.userId, refreshToken);
      if (!storedToken) {
        throw new Error('Invalid refresh token');
      }
      
      // Get user data
      const user = await this.userService.getUser(decoded.userId);
      if (!user) {
        throw new Error('User not found');
      }
      
      // Generate new tokens
      const newTokens = await this.generateTokens(user);
      
      // Blacklist old refresh token
      await this.blacklistRefreshToken(decoded.userId, refreshToken);
      
      return newTokens;
    } catch (error) {
      throw new Error('Invalid refresh token');
    }
  }
  
  async blacklistToken(token: string, reason: string = 'logout'): Promise<void> {
    const decoded = jwt.decode(token) as any;
    const expiry = decoded.exp * 1000; // Convert to milliseconds
    
    await this.redis.setex(
      `blacklist:${token}`,
      Math.floor((expiry - Date.now()) / 1000),
      JSON.stringify({ reason, blacklistedAt: new Date() })
    );
  }
  
  private async storeRefreshToken(userId: string, refreshToken: string): Promise<void> {
    const tokenHash = crypto.createHash('sha256').update(refreshToken).digest('hex');
    
    await this.database.query(`
      INSERT INTO refresh_tokens (user_id, token_hash, expires_at, created_at)
      VALUES ($1, $2, $3, $4)
    `, [userId, tokenHash, new Date(Date.now() + 7 * 24 * 60 * 60 * 1000), new Date()]);
  }
  
  private async getRefreshToken(userId: string, refreshToken: string): Promise<any> {
    const tokenHash = crypto.createHash('sha256').update(refreshToken).digest('hex');
    
    const result = await this.database.query(`
      SELECT * FROM refresh_tokens 
      WHERE user_id = $1 AND token_hash = $2 AND expires_at > NOW() AND revoked = false
    `, [userId, tokenHash]);
    
    return result.rows[0];
  }
}
```

### Authentication Controller
```typescript
@Controller('auth')
export class AuthController {
  constructor(
    private authService: AuthService,
    private jwtService: JWTService,
    private mfaService: MFAService,
    private auditService: AuditService
  ) {}
  
  @Post('login')
  async login(@Body() loginDto: LoginDto, @Req() req: Request) {
    try {
      // Validate credentials
      const user = await this.authService.validateCredentials(
        loginDto.email,
        loginDto.password
      );
      
      // Check if MFA is required
      if (user.mfaEnabled) {
        const mfaToken = await this.mfaService.generateMFAToken(user.id);
        
        await this.auditService.logEvent({
          userId: user.id,
          action: 'LOGIN_MFA_REQUIRED',
          ipAddress: req.ip,
          userAgent: req.get('User-Agent'),
          metadata: { email: loginDto.email }
        });
        
        return {
          success: true,
          mfaRequired: true,
          mfaToken: mfaToken
        };
      }
      
      // Generate tokens
      const tokens = await this.jwtService.generateTokens(user);
      
      // Log successful login
      await this.auditService.logEvent({
        userId: user.id,
        action: 'LOGIN_SUCCESS',
        ipAddress: req.ip,
        userAgent: req.get('User-Agent'),
        metadata: { email: loginDto.email }
      });
      
      return {
        success: true,
        accessToken: tokens.accessToken,
        refreshToken: tokens.refreshToken,
        user: {
          id: user.id,
          email: user.email,
          role: user.role,
          name: user.name
        }
      };
    } catch (error) {
      await this.auditService.logEvent({
        userId: null,
        action: 'LOGIN_FAILED',
        ipAddress: req.ip,
        userAgent: req.get('User-Agent'),
        metadata: { email: loginDto.email, error: error.message }
      });
      
      throw new UnauthorizedException('Invalid credentials');
    }
  }
  
  @Post('mfa/verify')
  async verifyMFA(@Body() mfaDto: MFADto, @Req() req: Request) {
    const isValid = await this.mfaService.verifyMFAToken(
      mfaDto.mfaToken,
      mfaDto.code
    );
    
    if (!isValid) {
      await this.auditService.logEvent({
        userId: null,
        action: 'MFA_FAILED',
        ipAddress: req.ip,
        userAgent: req.get('User-Agent'),
        metadata: { mfaToken: mfaDto.mfaToken }
      });
      
      throw new UnauthorizedException('Invalid MFA code');
    }
    
    const user = await this.userService.getUserByMFAToken(mfaDto.mfaToken);
    const tokens = await this.jwtService.generateTokens(user);
    
    await this.auditService.logEvent({
      userId: user.id,
      action: 'LOGIN_SUCCESS',
      ipAddress: req.ip,
      userAgent: req.get('User-Agent'),
      metadata: { email: user.email }
    });
    
    return {
      success: true,
      accessToken: tokens.accessToken,
      refreshToken: tokens.refreshToken,
      user: {
        id: user.id,
        email: user.email,
        role: user.role,
        name: user.name
      }
    };
  }
  
  @Post('refresh')
  async refreshToken(@Body() refreshDto: RefreshDto) {
    try {
      const tokens = await this.jwtService.refreshAccessToken(refreshDto.refreshToken);
      
      return {
        success: true,
        accessToken: tokens.accessToken,
        refreshToken: tokens.refreshToken
      };
    } catch (error) {
      throw new UnauthorizedException('Invalid refresh token');
    }
  }
  
  @Post('logout')
  @UseGuards(JwtAuthGuard)
  async logout(@Req() req: Request, @Body() logoutDto: LogoutDto) {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (token) {
      await this.jwtService.blacklistToken(token, 'logout');
    }
    
    if (logoutDto.refreshToken) {
      await this.jwtService.blacklistRefreshToken(req.user.userId, logoutDto.refreshToken);
    }
    
    await this.auditService.logEvent({
      userId: req.user.userId,
      action: 'LOGOUT',
      ipAddress: req.ip,
      userAgent: req.get('User-Agent')
    });
    
    return { success: true };
  }
}
```

## ROLE-BASED ACCESS CONTROL

### RBAC Service Implementation
```typescript
interface Permission {
  id: string;
  name: string;
  resource: string;
  action: string;
  conditions?: Record<string, any>;
}

interface Role {
  id: string;
  name: string;
  permissions: string[];
  parentRole?: string;
  universityId?: string;
  departmentId?: string;
}

class RBACService {
  async getUserPermissions(userId: string): Promise<string[]> {
    const user = await this.userService.getUser(userId);
    const userRoles = await this.getUserRoles(userId);
    
    const permissions = new Set<string>();
    
    for (const role of userRoles) {
      const rolePermissions = await this.getRolePermissions(role.id);
      rolePermissions.forEach(permission => permissions.add(permission));
    }
    
    // Add university-specific permissions
    if (user.universityId) {
      const universityPermissions = await this.getUniversityPermissions(user.universityId);
      universityPermissions.forEach(permission => permissions.add(permission));
    }
    
    // Add department-specific permissions
    if (user.departmentId) {
      const departmentPermissions = await this.getDepartmentPermissions(user.departmentId);
      departmentPermissions.forEach(permission => permissions.add(permission));
    }
    
    return Array.from(permissions);
  }
  
  async checkPermission(userId: string, resource: string, action: string, context?: any): Promise<boolean> {
    const permissions = await this.getUserPermissions(userId);
    const requiredPermission = `${resource}:${action}`;
    
    if (!permissions.includes(requiredPermission)) {
      return false;
    }
    
    // Check conditional permissions
    const conditionalPermissions = await this.getConditionalPermissions(userId, resource, action);
    
    for (const permission of conditionalPermissions) {
      if (permission.conditions) {
        const isAllowed = await this.evaluateConditions(permission.conditions, context);
        if (!isAllowed) {
          return false;
        }
      }
    }
    
    return true;
  }
  
  private async evaluateConditions(conditions: Record<string, any>, context: any): Promise<boolean> {
    // Example conditions evaluation
    if (conditions.ownerOnly && context.ownerId !== context.userId) {
      return false;
    }
    
    if (conditions.universityOnly && context.universityId !== context.userUniversityId) {
      return false;
    }
    
    if (conditions.departmentOnly && context.departmentId !== context.userDepartmentId) {
      return false;
    }
    
    if (conditions.examPeriodOnly) {
      const isExamPeriod = await this.isExamPeriod(context.universityId);
      if (!isExamPeriod) {
        return false;
      }
    }
    
    return true;
  }
  
  async createRole(roleData: Partial<Role>): Promise<Role> {
    const role = await this.database.query(`
      INSERT INTO roles (name, permissions, parent_role, university_id, department_id)
      VALUES ($1, $2, $3, $4, $5)
      RETURNING *
    `, [
      roleData.name,
      JSON.stringify(roleData.permissions),
      roleData.parentRole,
      roleData.universityId,
      roleData.departmentId
    ]);
    
    return role.rows[0];
  }
  
  async assignRoleToUser(userId: string, roleId: string): Promise<void> {
    await this.database.query(`
      INSERT INTO user_roles (user_id, role_id, assigned_at)
      VALUES ($1, $2, $3)
      ON CONFLICT (user_id, role_id) DO NOTHING
    `, [userId, roleId, new Date()]);
  }
}
```

### RBAC Guard Implementation
```typescript
@Injectable()
export class RBACGuard implements CanActivate {
  constructor(
    private rbacService: RBACService,
    private reflector: Reflector
  ) {}
  
  async canActivate(context: ExecutionContext): Promise<boolean> {
    const requiredPermissions = this.reflector.get<string[]>(
      'permissions',
      context.getHandler()
    );
    
    if (!requiredPermissions) {
      return true;
    }
    
    const request = context.switchToHttp().getRequest();
    const user = request.user;
    
    if (!user) {
      return false;
    }
    
    for (const permission of requiredPermissions) {
      const [resource, action] = permission.split(':');
      
      const hasPermission = await this.rbacService.checkPermission(
        user.userId,
        resource,
        action,
        {
          userId: user.userId,
          universityId: user.universityId,
          departmentId: user.departmentId,
          ...request.body,
          ...request.params,
          ...request.query
        }
      );
      
      if (!hasPermission) {
        return false;
      }
    }
    
    return true;
  }
}

// Usage in controllers
@Controller('rooms')
export class RoomController {
  @Get()
  @UseGuards(JwtAuthGuard, RBACGuard)
  @RequirePermissions(['rooms:read'])
  async getRooms() {
    // Implementation
  }
  
  @Post()
  @UseGuards(JwtAuthGuard, RBACGuard)
  @RequirePermissions(['rooms:create'])
  async createRoom() {
    // Implementation
  }
  
  @Put(':id')
  @UseGuards(JwtAuthGuard, RBACGuard)
  @RequirePermissions(['rooms:update'])
  async updateRoom() {
    // Implementation
  }
  
  @Delete(':id')
  @UseGuards(JwtAuthGuard, RBACGuard)
  @RequirePermissions(['rooms:delete'])
  async deleteRoom() {
    // Implementation
  }
}
```

## MULTI-FACTOR AUTHENTICATION

### MFA Service Implementation
```typescript
import speakeasy from 'speakeasy';
import QRCode from 'qrcode';

class MFAService {
  async generateMFASecret(userId: string): Promise<{ secret: string; qrCode: string }> {
    const secret = speakeasy.generateSecret({
      name: 'UniNest',
      issuer: 'UniNest',
      length: 32
    });
    
    const qrCode = await QRCode.toDataURL(secret.otpauth_url);
    
    // Store secret temporarily for verification
    await this.redis.setex(
      `mfa_setup:${userId}`,
      300, // 5 minutes
      secret.base32
    );
    
    return {
      secret: secret.base32,
      qrCode
    };
  }
  
  async verifyMFASetup(userId: string, token: string): Promise<boolean> {
    const secret = await this.redis.get(`mfa_setup:${userId}`);
    
    if (!secret) {
      throw new Error('MFA setup session expired');
    }
    
    const isValid = speakeasy.totp.verify({
      secret,
      encoding: 'base32',
      token,
      window: 2 // Allow 2 time steps (60 seconds) for clock skew
    });
    
    if (isValid) {
      // Store secret permanently
      await this.userService.updateMFASecret(userId, secret);
      await this.redis.del(`mfa_setup:${userId}`);
    }
    
    return isValid;
  }
  
  async generateMFAToken(userId: string): Promise<string> {
    const token = crypto.randomUUID();
    
    await this.redis.setex(
      `mfa_token:${token}`,
      300, // 5 minutes
      userId
    );
    
    return token;
  }
  
  async verifyMFAToken(mfaToken: string, code: string): Promise<boolean> {
    const userId = await this.redis.get(`mfa_token:${mfaToken}`);
    
    if (!userId) {
      throw new Error('MFA token expired');
    }
    
    const user = await this.userService.getUser(userId);
    const secret = user.mfaSecret;
    
    const isValid = speakeasy.totp.verify({
      secret,
      encoding: 'base32',
      token: code,
      window: 2
    });
    
    if (isValid) {
      await this.redis.del(`mfa_token:${mfaToken}`);
    }
    
    return isValid;
  }
  
  async generateBackupCodes(userId: string): Promise<string[]> {
    const codes = Array.from({ length: 10 }, () => 
      crypto.randomBytes(4).toString('hex').toUpperCase()
    );
    
    const hashedCodes = await Promise.all(
      codes.map(code => bcrypt.hash(code, 12))
    );
    
    await this.userService.updateBackupCodes(userId, hashedCodes);
    
    return codes;
  }
  
  async verifyBackupCode(userId: string, code: string): Promise<boolean> {
    const user = await this.userService.getUser(userId);
    const backupCodes = user.backupCodes || [];
    
    for (let i = 0; i < backupCodes.length; i++) {
      const isValid = await bcrypt.compare(code, backupCodes[i]);
      if (isValid) {
        // Remove used backup code
        backupCodes.splice(i, 1);
        await this.userService.updateBackupCodes(userId, backupCodes);
        return true;
      }
    }
    
    return false;
  }
}
```

## SECURITY MONITORING

### Audit Service Implementation
```typescript
interface AuditEvent {
  id?: string;
  userId?: string;
  action: string;
  ipAddress: string;
  userAgent: string;
  metadata?: Record<string, any>;
  timestamp?: Date;
  severity: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
}

class AuditService {
  async logEvent(event: AuditEvent): Promise<void> {
    const auditEvent = {
      ...event,
      id: crypto.randomUUID(),
      timestamp: new Date()
    };
    
    // Store in database
    await this.database.query(`
      INSERT INTO audit_logs (id, user_id, action, ip_address, user_agent, metadata, severity, created_at)
      VALUES ($1, $2, $3, $4, $5, $6, $7, $8)
    `, [
      auditEvent.id,
      auditEvent.userId,
      auditEvent.action,
      auditEvent.ipAddress,
      auditEvent.userAgent,
      JSON.stringify(auditEvent.metadata),
      auditEvent.severity,
      auditEvent.timestamp
    ]);
    
    // Send to monitoring system for high severity events
    if (event.severity === 'HIGH' || event.severity === 'CRITICAL') {
      await this.sendSecurityAlert(auditEvent);
    }
  }
  
  async detectAnomalies(userId: string): Promise<SecurityAlert[]> {
    const recentEvents = await this.getRecentEvents(userId, 24 * 60 * 60 * 1000); // Last 24 hours
    
    const alerts: SecurityAlert[] = [];
    
    // Check for multiple failed login attempts
    const failedLogins = recentEvents.filter(e => e.action === 'LOGIN_FAILED');
    if (failedLogins.length > 5) {
      alerts.push({
        type: 'MULTIPLE_FAILED_LOGINS',
        userId,
        severity: 'HIGH',
        description: `${failedLogins.length} failed login attempts detected`
      });
    }
    
    // Check for login from new location
    const loginEvents = recentEvents.filter(e => e.action === 'LOGIN_SUCCESS');
    const locations = new Set(loginEvents.map(e => e.ipAddress));
    if (locations.size > 3) {
      alerts.push({
        type: 'MULTIPLE_LOCATIONS',
        userId,
        severity: 'MEDIUM',
        description: `Login from ${locations.size} different locations`
      });
    }
    
    // Check for unusual activity patterns
    const activityCount = recentEvents.length;
    const avgActivity = await this.getAverageActivity(userId);
    if (activityCount > avgActivity * 3) {
      alerts.push({
        type: 'UNUSUAL_ACTIVITY',
        userId,
        severity: 'MEDIUM',
        description: `Unusual activity level detected`
      });
    }
    
    return alerts;
  }
  
  private async sendSecurityAlert(alert: SecurityAlert): Promise<void> {
    // Send email notification
    await this.emailService.sendSecurityAlert(alert);
    
    // Send SMS notification for critical alerts
    if (alert.severity === 'CRITICAL') {
      await this.smsService.sendSecurityAlert(alert);
    }
    
    // Log to security monitoring system
    await this.securityMonitoringService.logAlert(alert);
  }
}
```

## SESSION MANAGEMENT

### Session Service Implementation
```typescript
interface UserSession {
  id: string;
  userId: string;
  deviceId: string;
  deviceName: string;
  deviceType: 'web' | 'mobile' | 'tablet';
  ipAddress: string;
  userAgent: string;
  lastActive: Date;
  isActive: boolean;
  trusted: boolean;
}

class SessionService {
  async createSession(userId: string, deviceInfo: any): Promise<UserSession> {
    const session: UserSession = {
      id: crypto.randomUUID(),
      userId,
      deviceId: deviceInfo.deviceId,
      deviceName: deviceInfo.deviceName,
      deviceType: deviceInfo.deviceType,
      ipAddress: deviceInfo.ipAddress,
      userAgent: deviceInfo.userAgent,
      lastActive: new Date(),
      isActive: true,
      trusted: false
    };
    
    await this.database.query(`
      INSERT INTO user_sessions (id, user_id, device_id, device_name, device_type, ip_address, user_agent, last_active, is_active, trusted)
      VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10)
    `, [
      session.id, session.userId, session.deviceId, session.deviceName,
      session.deviceType, session.ipAddress, session.userAgent,
      session.lastActive, session.isActive, session.trusted
    ]);
    
    return session;
  }
  
  async updateSessionActivity(sessionId: string): Promise<void> {
    await this.database.query(`
      UPDATE user_sessions 
      SET last_active = NOW()
      WHERE id = $1
    `, [sessionId]);
  }
  
  async getUserSessions(userId: string): Promise<UserSession[]> {
    const result = await this.database.query(`
      SELECT * FROM user_sessions 
      WHERE user_id = $1 AND is_active = true
      ORDER BY last_active DESC
    `, [userId]);
    
    return result.rows;
  }
  
  async revokeSession(sessionId: string): Promise<void> {
    await this.database.query(`
      UPDATE user_sessions 
      SET is_active = false, revoked_at = NOW()
      WHERE id = $1
    `, [sessionId]);
  }
  
  async revokeAllSessions(userId: string, exceptSessionId?: string): Promise<void> {
    let query = `
      UPDATE user_sessions 
      SET is_active = false, revoked_at = NOW()
      WHERE user_id = $1
    `;
    
    const params = [userId];
    
    if (exceptSessionId) {
      query += ` AND id != $2`;
      params.push(exceptSessionId);
    }
    
    await this.database.query(query, params);
  }
  
  async markSessionAsTrusted(sessionId: string): Promise<void> {
    await this.database.query(`
      UPDATE user_sessions 
      SET trusted = true
      WHERE id = $1
    `, [sessionId]);
  }
}
```

## íž¨ CREATIVE CHECKPOINT: AUTHENTICATION SYSTEM DESIGN COMPLETE

**Decision Made**: Advanced JWT with Comprehensive RBAC and MFA
**Key Features**: 
- JWT-based authentication with refresh token rotation
- Comprehensive role-based access control with granular permissions
- Multi-factor authentication with TOTP and backup codes
- Advanced session management with device tracking
- Real-time security monitoring and threat detection
- Comprehensive audit logging and compliance features

**Next Creative Phase**: File Storage System Design

## íž¨íž¨íž¨ EXITING CREATIVE PHASE - DECISION MADE íž¨íž¨íž¨

*This creative phase document captures the authentication system design decisions for UniNest*
