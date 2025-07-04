# ��� CREATIVE PHASE: File Storage System Design

## ��������� ENTERING CREATIVE PHASE: FILE STORAGE SYSTEM ���������

**Date:** $(date)  
**Phase:** 8 of 8 - File Storage System Design  
**Type:** Infrastructure Design  
**Focus:** AWS S3 integration, image processing, and file management for room photos and documents

## PROBLEM STATEMENT

Design a comprehensive file storage system for UniNest that supports:
1. **Room photo management** with multiple image uploads and optimization
2. **Document storage** for contracts, verification documents, and certificates
3. **Image processing** with automatic resizing, compression, and format conversion
4. **CDN integration** for fast global content delivery
5. **File security** with access control and encryption
6. **Backup and redundancy** for data protection
7. **Cost optimization** with intelligent storage tiering
8. **Scalable architecture** for high-volume file operations

**Key Challenges:**
- High-volume image uploads from multiple users simultaneously
- Image optimization for different devices and network conditions
- Secure file access with role-based permissions
- Cost-effective storage with intelligent tiering
- CDN integration for global performance
- Backup and disaster recovery strategies
- File versioning and metadata management
- Integration with room listing and user profile systems

## OPTIONS ANALYSIS

### Option 1: Simple Local Storage with Basic Processing
**Description**: Local file system storage with basic image processing
**Pros**:
- Simple implementation and easy to understand
- No external dependencies or costs
- Full control over file storage
- Quick to implement and deploy
- No network latency for file operations
**Cons**:
- Limited scalability and storage capacity
- No built-in redundancy or backup
- Manual CDN setup required
- Limited image processing capabilities
- Single point of failure
**Complexity**: Low
**Implementation Time**: 2-3 weeks

### Option 2: Cloud Storage with Advanced Processing
**Description**: AWS S3 with CloudFront CDN and comprehensive image processing
**Pros**:
- Highly scalable and reliable
- Built-in redundancy and backup
- Global CDN for fast delivery
- Advanced image processing capabilities
- Cost-effective with intelligent tiering
- Automatic scaling and management
**Cons**:
- More complex implementation
- External dependency on AWS
- Ongoing cloud costs
- Requires AWS expertise
- Network latency for uploads
**Complexity**: Medium-High
**Implementation Time**: 6-8 weeks

### Option 3: Hybrid Multi-Cloud Storage
**Description**: Multi-cloud storage with advanced processing and redundancy
**Pros**:
- Maximum reliability and redundancy
- Vendor lock-in avoidance
- Best performance optimization
- Advanced security features
- Global distribution capabilities
- Future-proof architecture
**Cons**:
- Most complex implementation
- Highest development and operational costs
- Multiple vendor management
- Complex synchronization
- Requires expert knowledge
**Complexity**: High
**Implementation Time**: 10-12 weeks

## DECISION

**Selected Approach**: **Option 2 - Cloud Storage with Advanced Processing**

**Rationale**:
1. **MVP Requirements**: Balances functionality with implementation complexity
2. **Scalability**: AWS S3 provides excellent scaling capabilities
3. **Cost-Effectiveness**: Intelligent tiering reduces storage costs
4. **Performance**: CloudFront CDN ensures fast global delivery
5. **Reliability**: Built-in redundancy and backup features
6. **Future Growth**: Architecture supports scaling to multiple regions

## IMPLEMENTATION PLAN

### Phase 1: Core Storage Infrastructure (Week 1-2)
```
AWS S3 Setup
├── S3 bucket configuration with versioning
├── CloudFront CDN distribution
├── IAM roles and permissions
├── CORS configuration
└── Lifecycle policies for cost optimization

Database Schema
├── files table for metadata storage
├── file_versions table for versioning
├── file_permissions table for access control
└── file_processing_jobs table for background tasks
```

### Phase 2: File Upload and Processing (Week 3-4)
```
Upload System
├── Multipart upload for large files
├── File validation and virus scanning
├── Image processing pipeline
├── Thumbnail generation
└── Metadata extraction

Processing Pipeline
├── Image resizing and optimization
├── Format conversion (WebP, AVIF)
├── Quality optimization
├── EXIF data handling
└── Background job processing
```

### Phase 3: Access Control and Security (Week 5-6)
```
Security Implementation
├── Signed URLs for secure access
├── Role-based file permissions
├── File encryption at rest
├── Access logging and monitoring
└── Virus scanning integration

CDN Optimization
├── Cache optimization strategies
├── Geographic distribution
├── Compression and optimization
├── Error handling and fallbacks
└── Performance monitoring
```

### Phase 4: Advanced Features and Optimization (Week 7-8)
```
Advanced Features
├── File versioning and rollback
├── Bulk operations and batch processing
├── Storage analytics and reporting
├── Cost optimization and monitoring
└── Backup and disaster recovery

Performance Optimization
├── Upload optimization
├── Download optimization
├── Cache strategies
├── Load balancing
└── Monitoring and alerting
```

## VISUALIZATION

### File Storage System Architecture
```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │   Web Client    │  │  Mobile Client  │  │  Admin Client   │  │
│  │   (React)       │  │   (Android)     │  │   (Dashboard)   │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                │
                      ┌─────────▼─────────┐
                      │   API GATEWAY     │
                      │   (File Upload)   │
                      └─────────┬─────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                    PROCESSING LAYER                             │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │   Upload    │ │   Image     │ │   File      │ │   Metadata  │ │
│ │   Handler   │ │   Processor │ │   Validator │ │   Extractor │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                      STORAGE LAYER                              │
│ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────────┐ │
│ │   AWS S3        │ │   CloudFront    │ │   Database          │ │
│ │   (Primary)     │ │   (CDN)         │ │   (Metadata)        │ │
│ └─────────────────┘ └─────────────────┘ └─────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### File Upload Flow Diagram
```
File Upload Process
├── Client initiates upload
├── Generate presigned URL
├── Client uploads to S3
├── S3 triggers Lambda function
├── Process image (resize, optimize)
├── Generate thumbnails
├── Extract metadata
├── Store metadata in database
├── Update CDN cache
└── Return file URLs to client

File Access Process
├── Client requests file
├── Check permissions
├── Generate signed URL
├── Redirect to CDN
├── CDN serves optimized file
└── Log access for analytics
```

## AWS S3 IMPLEMENTATION

### S3 Configuration and Setup
```typescript
import { S3Client, PutObjectCommand, GetObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';

class S3Service {
  private s3Client: S3Client;
  private bucketName: string;
  private region: string;
  
  constructor() {
    this.s3Client = new S3Client({
      region: process.env.AWS_REGION,
      credentials: {
        accessKeyId: process.env.AWS_ACCESS_KEY_ID,
        secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY
      }
    });
    
    this.bucketName = process.env.S3_BUCKET_NAME;
    this.region = process.env.AWS_REGION;
  }
  
  async generatePresignedUploadUrl(
    key: string,
    contentType: string,
    expiresIn: number = 3600
  ): Promise<string> {
    const command = new PutObjectCommand({
      Bucket: this.bucketName,
      Key: key,
      ContentType: contentType,
      Metadata: {
        'upload-timestamp': new Date().toISOString()
      }
    });
    
    return await getSignedUrl(this.s3Client, command, { expiresIn });
  }
  
  async generatePresignedDownloadUrl(
    key: string,
    expiresIn: number = 3600
  ): Promise<string> {
    const command = new GetObjectCommand({
      Bucket: this.bucketName,
      Key: key
    });
    
    return await getSignedUrl(this.s3Client, command, { expiresIn });
  }
  
  async uploadFile(
    key: string,
    fileBuffer: Buffer,
    contentType: string,
    metadata?: Record<string, string>
  ): Promise<void> {
    const command = new PutObjectCommand({
      Bucket: this.bucketName,
      Key: key,
      Body: fileBuffer,
      ContentType: contentType,
      Metadata: metadata,
      ServerSideEncryption: 'AES256'
    });
    
    await this.s3Client.send(command);
  }
  
  async getFileMetadata(key: string): Promise<any> {
    const command = new GetObjectCommand({
      Bucket: this.bucketName,
      Key: key
    });
    
    const response = await this.s3Client.send(command);
    return {
      contentType: response.ContentType,
      contentLength: response.ContentLength,
      lastModified: response.LastModified,
      metadata: response.Metadata
    };
  }
  
  async deleteFile(key: string): Promise<void> {
    const command = new DeleteObjectCommand({
      Bucket: this.bucketName,
      Key: key
    });
    
    await this.s3Client.send(command);
  }
}
```

### File Upload Controller
```typescript
@Controller('files')
export class FileController {
  constructor(
    private fileService: FileService,
    private s3Service: S3Service,
    private imageProcessor: ImageProcessor,
    private permissionService: PermissionService
  ) {}
  
  @Post('upload/initiate')
  @UseGuards(JwtAuthGuard)
  async initiateUpload(
    @Body() uploadDto: InitiateUploadDto,
    @Req() req: Request
  ) {
    const { fileName, fileSize, contentType, roomId } = uploadDto;
    
    // Validate file type and size
    await this.fileService.validateFile(fileName, fileSize, contentType);
    
    // Check permissions
    if (roomId) {
      const hasPermission = await this.permissionService.checkPermission(
        req.user.userId,
        'rooms',
        'update',
        { roomId }
      );
      
      if (!hasPermission) {
        throw new ForbiddenException('No permission to upload files for this room');
      }
    }
    
    // Generate unique file key
    const fileKey = this.generateFileKey(fileName, req.user.userId);
    
    // Generate presigned URL
    const presignedUrl = await this.s3Service.generatePresignedUploadUrl(
      fileKey,
      contentType
    );
    
    // Create file record in database
    const fileRecord = await this.fileService.createFileRecord({
      key: fileKey,
      fileName,
      fileSize,
      contentType,
      uploadedBy: req.user.userId,
      roomId,
      status: 'uploading'
    });
    
    return {
      uploadId: fileRecord.id,
      presignedUrl,
      fileKey,
      expiresIn: 3600
    };
  }
  
  @Post('upload/complete')
  @UseGuards(JwtAuthGuard)
  async completeUpload(
    @Body() completeDto: CompleteUploadDto,
    @Req() req: Request
  ) {
    const { uploadId, etag } = completeDto;
    
    // Get file record
    const fileRecord = await this.fileService.getFileRecord(uploadId);
    
    if (!fileRecord || fileRecord.uploadedBy !== req.user.userId) {
      throw new NotFoundException('Upload not found');
    }
    
    // Verify upload completion
    const metadata = await this.s3Service.getFileMetadata(fileRecord.key);
    
    // Update file record
    await this.fileService.updateFileRecord(uploadId, {
      status: 'uploaded',
      etag,
      uploadedAt: new Date()
    });
    
    // Trigger image processing if it's an image
    if (this.isImageFile(fileRecord.contentType)) {
      await this.imageProcessor.processImage(fileRecord.key);
    }
    
    // Generate access URLs
    const urls = await this.generateFileUrls(fileRecord);
    
    return {
      success: true,
      fileId: fileRecord.id,
      urls
    };
  }
  
  @Get(':fileId')
  @UseGuards(JwtAuthGuard)
  async getFile(
    @Param('fileId') fileId: string,
    @Req() req: Request,
    @Query() query: GetFileQuery
  ) {
    const fileRecord = await this.fileService.getFileRecord(fileId);
    
    if (!fileRecord) {
      throw new NotFoundException('File not found');
    }
    
    // Check permissions
    const hasPermission = await this.permissionService.checkFilePermission(
      req.user.userId,
      fileRecord
    );
    
    if (!hasPermission) {
      throw new ForbiddenException('No permission to access this file');
    }
    
    // Generate signed URL
    const signedUrl = await this.s3Service.generatePresignedDownloadUrl(
      fileRecord.key,
      query.expiresIn || 3600
    );
    
    // Log access
    await this.fileService.logFileAccess(fileId, req.user.userId, req.ip);
    
    return {
      file: fileRecord,
      downloadUrl: signedUrl,
      cdnUrl: this.generateCDNUrl(fileRecord.key, query.size)
    };
  }
  
  @Delete(':fileId')
  @UseGuards(JwtAuthGuard)
  async deleteFile(
    @Param('fileId') fileId: string,
    @Req() req: Request
  ) {
    const fileRecord = await this.fileService.getFileRecord(fileId);
    
    if (!fileRecord) {
      throw new NotFoundException('File not found');
    }
    
    // Check permissions
    const hasPermission = await this.permissionService.checkFilePermission(
      req.user.userId,
      fileRecord,
      'delete'
    );
    
    if (!hasPermission) {
      throw new ForbiddenException('No permission to delete this file');
    }
    
    // Delete from S3
    await this.s3Service.deleteFile(fileRecord.key);
    
    // Delete from database
    await this.fileService.deleteFileRecord(fileId);
    
    return { success: true };
  }
  
  private generateFileKey(fileName: string, userId: string): string {
    const timestamp = Date.now();
    const randomId = crypto.randomUUID();
    const extension = path.extname(fileName);
    const baseName = path.basename(fileName, extension);
    
    return `uploads/${userId}/${timestamp}-${randomId}-${baseName}${extension}`;
  }
  
  private isImageFile(contentType: string): boolean {
    return contentType.startsWith('image/');
  }
  
  private generateCDNUrl(key: string, size?: string): string {
    const cdnDomain = process.env.CLOUDFRONT_DOMAIN;
    const url = `https://${cdnDomain}/${key}`;
    
    if (size) {
      return `${url}?size=${size}`;
    }
    
    return url;
  }
}
```

## IMAGE PROCESSING PIPELINE

### Image Processor Service
```typescript
import sharp from 'sharp';
import { S3Client, GetObjectCommand, PutObjectCommand } from '@aws-sdk/client-s3';

interface ImageProcessingOptions {
  width?: number;
  height?: number;
  quality?: number;
  format?: 'jpeg' | 'webp' | 'avif';
  fit?: 'cover' | 'contain' | 'fill';
}

class ImageProcessor {
  private s3Client: S3Client;
  private bucketName: string;
  
  constructor() {
    this.s3Client = new S3Client({
      region: process.env.AWS_REGION,
      credentials: {
        accessKeyId: process.env.AWS_ACCESS_KEY_ID,
        secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY
      }
    });
    
    this.bucketName = process.env.S3_BUCKET_NAME;
  }
  
  async processImage(fileKey: string): Promise<void> {
    try {
      // Download original image from S3
      const originalImage = await this.downloadImage(fileKey);
      
      // Process image for different sizes
      const sizes = [
        { name: 'thumbnail', width: 150, height: 150 },
        { name: 'small', width: 400, height: 300 },
        { name: 'medium', width: 800, height: 600 },
        { name: 'large', width: 1200, height: 900 }
      ];
      
      const processingPromises = sizes.map(size => 
        this.createImageVariant(fileKey, originalImage, size)
      );
      
      await Promise.all(processingPromises);
      
      // Update file record with processing status
      await this.updateProcessingStatus(fileKey, 'completed');
      
    } catch (error) {
      console.error('Image processing failed:', error);
      await this.updateProcessingStatus(fileKey, 'failed', error.message);
      throw error;
    }
  }
  
  private async downloadImage(fileKey: string): Promise<Buffer> {
    const command = new GetObjectCommand({
      Bucket: this.bucketName,
      Key: fileKey
    });
    
    const response = await this.s3Client.send(command);
    const chunks: Buffer[] = [];
    
    for await (const chunk of response.Body as any) {
      chunks.push(chunk);
    }
    
    return Buffer.concat(chunks);
  }
  
  private async createImageVariant(
    originalKey: string,
    imageBuffer: Buffer,
    size: { name: string; width: number; height: number }
  ): Promise<void> {
    const variantKey = this.generateVariantKey(originalKey, size.name);
    
    // Process image with Sharp
    const processedImage = await sharp(imageBuffer)
      .resize(size.width, size.height, {
        fit: 'cover',
        position: 'center'
      })
      .jpeg({ quality: 85, progressive: true })
      .toBuffer();
    
    // Upload processed image
    const command = new PutObjectCommand({
      Bucket: this.bucketName,
      Key: variantKey,
      Body: processedImage,
      ContentType: 'image/jpeg',
      CacheControl: 'public, max-age=31536000', // 1 year
      Metadata: {
        'original-key': originalKey,
        'variant': size.name,
        'processed-at': new Date().toISOString()
      }
    });
    
    await this.s3Client.send(command);
  }
  
  private generateVariantKey(originalKey: string, variant: string): string {
    const pathParts = originalKey.split('/');
    const fileName = pathParts.pop();
    const nameWithoutExt = fileName.split('.')[0];
    const extension = fileName.split('.').pop();
    
    return `${pathParts.join('/')}/${nameWithoutExt}-${variant}.${extension}`;
  }
  
  async optimizeImage(
    imageBuffer: Buffer,
    options: ImageProcessingOptions
  ): Promise<Buffer> {
    let sharpInstance = sharp(imageBuffer);
    
    // Resize if dimensions provided
    if (options.width || options.height) {
      sharpInstance = sharpInstance.resize(options.width, options.height, {
        fit: options.fit || 'cover'
      });
    }
    
    // Convert format and set quality
    if (options.format === 'webp') {
      return await sharpInstance.webp({ quality: options.quality || 85 }).toBuffer();
    } else if (options.format === 'avif') {
      return await sharpInstance.avif({ quality: options.quality || 85 }).toBuffer();
    } else {
      return await sharpInstance.jpeg({ quality: options.quality || 85 }).toBuffer();
    }
  }
  
  async extractMetadata(imageBuffer: Buffer): Promise<any> {
    const metadata = await sharp(imageBuffer).metadata();
    
    return {
      width: metadata.width,
      height: metadata.height,
      format: metadata.format,
      size: metadata.size,
      hasAlpha: metadata.hasAlpha,
      hasProfile: metadata.hasProfile,
      isOpaque: metadata.isOpaque,
      orientation: metadata.orientation
    };
  }
}
```

## CDN INTEGRATION

### CloudFront Configuration
```typescript
class CDNService {
  private cloudFrontDomain: string;
  private s3BucketName: string;
  
  constructor() {
    this.cloudFrontDomain = process.env.CLOUDFRONT_DOMAIN;
    this.s3BucketName = process.env.S3_BUCKET_NAME;
  }
  
  generateImageUrl(
    fileKey: string,
    size?: string,
    format?: string
  ): string {
    let url = `https://${this.cloudFrontDomain}/${fileKey}`;
    const params = new URLSearchParams();
    
    if (size) {
      params.append('size', size);
    }
    
    if (format) {
      params.append('format', format);
    }
    
    if (params.toString()) {
      url += `?${params.toString()}`;
    }
    
    return url;
  }
  
  async invalidateCache(paths: string[]): Promise<void> {
    // CloudFront invalidation would be implemented here
    // This requires AWS CloudFront SDK integration
    console.log('Invalidating cache for paths:', paths);
  }
  
  async warmCache(urls: string[]): Promise<void> {
    // Pre-warm cache by making requests to CDN
    const promises = urls.map(url => 
      fetch(url, { method: 'HEAD' })
    );
    
    await Promise.all(promises);
  }
  
  getCacheHeaders(fileType: string): Record<string, string> {
    const cacheHeaders: Record<string, string> = {
      'Cache-Control': 'public, max-age=31536000' // 1 year for static assets
    };
    
    if (fileType.startsWith('image/')) {
      cacheHeaders['Cache-Control'] = 'public, max-age=31536000, immutable';
    } else if (fileType === 'application/pdf') {
      cacheHeaders['Cache-Control'] = 'public, max-age=86400'; // 1 day for documents
    }
    
    return cacheHeaders;
  }
}
```

## FILE PERMISSIONS AND SECURITY

### File Permission Service
```typescript
class FilePermissionService {
  async checkFilePermission(
    userId: string,
    fileRecord: any,
    action: 'read' | 'write' | 'delete' = 'read'
  ): Promise<boolean> {
    // File owner has full permissions
    if (fileRecord.uploadedBy === userId) {
      return true;
    }
    
    // Check room-based permissions
    if (fileRecord.roomId) {
      const roomPermission = await this.checkRoomPermission(userId, fileRecord.roomId, action);
      if (roomPermission) {
        return true;
      }
    }
    
    // Check role-based permissions
    const userRole = await this.getUserRole(userId);
    if (userRole === 'admin' || userRole === 'university_official') {
      return true;
    }
    
    // Check university-based permissions
    if (fileRecord.universityId) {
      const universityPermission = await this.checkUniversityPermission(
        userId,
        fileRecord.universityId,
        action
      );
      if (universityPermission) {
        return true;
      }
    }
    
    return false;
  }
  
  private async checkRoomPermission(
    userId: string,
    roomId: string,
    action: string
  ): Promise<boolean> {
    const room = await this.roomService.getRoom(roomId);
    
    if (!room) {
      return false;
    }
    
    // Room owner has full permissions
    if (room.ownerId === userId) {
      return true;
    }
    
    // Check if user has booking for this room
    if (action === 'read') {
      const hasBooking = await this.bookingService.hasActiveBooking(userId, roomId);
      if (hasBooking) {
        return true;
      }
    }
    
    return false;
  }
  
  async setFilePermissions(
    fileId: string,
    permissions: FilePermission[]
  ): Promise<void> {
    await this.database.query(`
      DELETE FROM file_permissions WHERE file_id = $1
    `, [fileId]);
    
    for (const permission of permissions) {
      await this.database.query(`
        INSERT INTO file_permissions (file_id, user_id, role_id, permission, created_at)
        VALUES ($1, $2, $3, $4, $5)
      `, [fileId, permission.userId, permission.roleId, permission.permission, new Date()]);
    }
  }
  
  async getFilePermissions(fileId: string): Promise<FilePermission[]> {
    const result = await this.database.query(`
      SELECT * FROM file_permissions WHERE file_id = $1
    `, [fileId]);
    
    return result.rows;
  }
}
```

## STORAGE OPTIMIZATION

### Storage Analytics and Cost Management
```typescript
class StorageAnalyticsService {
  async getStorageMetrics(): Promise<StorageMetrics> {
    const metrics = await this.database.query(`
      SELECT 
        COUNT(*) as total_files,
        SUM(file_size) as total_size,
        AVG(file_size) as avg_file_size,
        COUNT(CASE WHEN content_type LIKE 'image/%' THEN 1 END) as image_count,
        COUNT(CASE WHEN content_type = 'application/pdf' THEN 1 END) as document_count
      FROM files 
      WHERE status = 'uploaded'
    `);
    
    return metrics.rows[0];
  }
  
  async getStorageByUser(userId: string): Promise<UserStorageMetrics> {
    const metrics = await this.database.query(`
      SELECT 
        COUNT(*) as file_count,
        SUM(file_size) as total_size,
        MAX(uploaded_at) as last_upload
      FROM files 
      WHERE uploaded_by = $1 AND status = 'uploaded'
    `, [userId]);
    
    return metrics.rows[0];
  }
  
  async getStorageByRoom(roomId: string): Promise<RoomStorageMetrics> {
    const metrics = await this.database.query(`
      SELECT 
        COUNT(*) as file_count,
        SUM(file_size) as total_size,
        COUNT(CASE WHEN content_type LIKE 'image/%' THEN 1 END) as image_count
      FROM files 
      WHERE room_id = $1 AND status = 'uploaded'
    `, [roomId]);
    
    return metrics.rows[0];
  }
  
  async cleanupOrphanedFiles(): Promise<number> {
    // Find files that are not associated with any room or user
    const orphanedFiles = await this.database.query(`
      SELECT f.* FROM files f
      LEFT JOIN rooms r ON f.room_id = r.id
      LEFT JOIN users u ON f.uploaded_by = u.id
      WHERE (f.room_id IS NOT NULL AND r.id IS NULL)
         OR (f.uploaded_by IS NOT NULL AND u.id IS NULL)
         OR f.status = 'failed'
    `);
    
    let deletedCount = 0;
    
    for (const file of orphanedFiles.rows) {
      try {
        await this.s3Service.deleteFile(file.key);
        await this.fileService.deleteFileRecord(file.id);
        deletedCount++;
      } catch (error) {
        console.error('Failed to delete orphaned file:', file.id, error);
      }
    }
    
    return deletedCount;
  }
  
  async optimizeStorage(): Promise<StorageOptimizationResult> {
    const result: StorageOptimizationResult = {
      filesOptimized: 0,
      spaceSaved: 0,
      errors: []
    };
    
    // Find large images that can be optimized
    const largeImages = await this.database.query(`
      SELECT * FROM files 
      WHERE content_type LIKE 'image/%' 
        AND file_size > 1048576 -- 1MB
        AND status = 'uploaded'
      ORDER BY file_size DESC
      LIMIT 100
    `);
    
    for (const image of largeImages.rows) {
      try {
        const originalSize = image.file_size;
        await this.imageProcessor.optimizeImage(image.key, {
          quality: 80,
          format: 'webp'
        });
        
        const newMetadata = await this.s3Service.getFileMetadata(image.key);
        const newSize = newMetadata.contentLength;
        
        result.filesOptimized++;
        result.spaceSaved += (originalSize - newSize);
        
        // Update file record
        await this.fileService.updateFileRecord(image.id, {
          fileSize: newSize,
          optimizedAt: new Date()
        });
        
      } catch (error) {
        result.errors.push({
          fileId: image.id,
          error: error.message
        });
      }
    }
    
    return result;
  }
}
```

## BACKUP AND DISASTER RECOVERY

### Backup Service Implementation
```typescript
class BackupService {
  async createBackup(): Promise<BackupResult> {
    const backupId = crypto.randomUUID();
    const timestamp = new Date().toISOString();
    
    try {
      // Backup file metadata
      const fileMetadata = await this.database.query(`
        SELECT * FROM files WHERE status = 'uploaded'
      `);
      
      // Create backup file
      const backupData = {
        id: backupId,
        timestamp,
        fileCount: fileMetadata.rows.length,
        files: fileMetadata.rows
      };
      
      const backupKey = `backups/${timestamp}/${backupId}.json`;
      await this.s3Service.uploadFile(
        backupKey,
        Buffer.from(JSON.stringify(backupData)),
        'application/json',
        { 'backup-type': 'metadata' }
      );
      
      // Log backup creation
      await this.database.query(`
        INSERT INTO backup_logs (backup_id, backup_type, file_count, created_at)
        VALUES ($1, $2, $3, $4)
      `, [backupId, 'metadata', fileMetadata.rows.length, new Date()]);
      
      return {
        success: true,
        backupId,
        fileCount: fileMetadata.rows.length,
        backupKey
      };
      
    } catch (error) {
      console.error('Backup creation failed:', error);
      return {
        success: false,
        error: error.message
      };
    }
  }
  
  async restoreFromBackup(backupId: string): Promise<RestoreResult> {
    try {
      // Download backup file
      const backupKey = `backups/${backupId}.json`;
      const backupData = await this.downloadBackup(backupKey);
      
      // Restore file metadata
      for (const file of backupData.files) {
        await this.database.query(`
          INSERT INTO files (id, key, file_name, file_size, content_type, uploaded_by, room_id, status, uploaded_at)
          VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9)
          ON CONFLICT (id) DO UPDATE SET
            key = EXCLUDED.key,
            file_name = EXCLUDED.file_name,
            file_size = EXCLUDED.file_size,
            content_type = EXCLUDED.content_type,
            status = EXCLUDED.status
        `, [
          file.id, file.key, file.file_name, file.file_size,
          file.content_type, file.uploaded_by, file.room_id,
          file.status, file.uploaded_at
        ]);
      }
      
      return {
        success: true,
        restoredFiles: backupData.files.length
      };
      
    } catch (error) {
      console.error('Restore failed:', error);
      return {
        success: false,
        error: error.message
      };
    }
  }
  
  private async downloadBackup(backupKey: string): Promise<any> {
    const command = new GetObjectCommand({
      Bucket: this.bucketName,
      Key: backupKey
    });
    
    const response = await this.s3Client.send(command);
    const chunks: Buffer[] = [];
    
    for await (const chunk of response.Body as any) {
      chunks.push(chunk);
    }
    
    const backupContent = Buffer.concat(chunks).toString('utf8');
    return JSON.parse(backupContent);
  }
}
```

## ��� CREATIVE CHECKPOINT: FILE STORAGE SYSTEM DESIGN COMPLETE

**Decision Made**: Cloud Storage with Advanced Processing
**Key Features**: 
- AWS S3 integration with CloudFront CDN for global delivery
- Comprehensive image processing pipeline with multiple formats
- Secure file access with role-based permissions and signed URLs
- Storage optimization with intelligent tiering and analytics
- Backup and disaster recovery with automated processes
- Cost-effective storage management with monitoring and cleanup

**All Creative Phases Complete**: 8/8 phases finished

## ��������� EXITING CREATIVE PHASE - DECISION MADE ���������

*This creative phase document captures the file storage system design decisions for UniNest*
